<%*
const { execFileSync } = require('child_process');
const fs = require('fs');
const path = require('path');
const vaultPath = app.vault.adapter?.basePath;

// --- הגדרות ---
const WATCH_FOLDER = "10 - Courses";
const HEADER = "## קבצים שנערכו היום";
const EMPTY_TREE = "4b825dc642cb6eb9a060e54bf8d69288fbee4904";

const escapePipes = (str) => String(str ?? '').replace(/\\\|/g, '|').replace(/\|/g, '\\|');

function runGit(args, gitOpts) {
  return execFileSync('git', ['-c', 'core.quotepath=false', ...args], gitOpts);
}

// נרמול קישורים (Wikilinks, Markdown links, ו-YAML Frontmatter)
const normalizeLine = (line) =>
  line
    .replace(/["']?\[\[[^\]]+\]\]["']?/g, '[[LINK]]')
    .replace(/!?\[[^\]]*\]\([^)]+\)/g, '[LINK](LINK)')
    .replace(/\s+/g, ' ')
    .trim();

const normalizeBlock = (lines) =>
  lines
    .map(normalizeLine)
    .join(' ')
    .replace(/(?:^|\s)(?:-\s+|\[\s*|\s*\])(?=\[\[LINK\]\])/g, ' ')
    .replace(/\[\[LINK\]\]\s*\]/g, '[[LINK]]')
    .replace(/\s+/g, ' ')
    .trim();

function isLinkOnlyChange(filePath, diffArgs, gitOpts) {
  try {
    const diff = runGit(['diff', '-U0', ...diffArgs, '--', filePath], gitOpts);
    const minusLines = [];
    const plusLines = [];

    for (const line of diff.split(/\r?\n/)) {
      if (line.startsWith('--- ') || line.startsWith('+++ ')) continue;
      if (line.startsWith('-')) minusLines.push(line.slice(1));
      else if (line.startsWith('+')) plusLines.push(line.slice(1));
    }

    if (minusLines.length === 0 && plusLines.length === 0) return true;

    if (
      minusLines.length === plusLines.length &&
      minusLines.every((mLine, i) => normalizeLine(mLine) === normalizeLine(plusLines[i]))
    ) {
      return true;
    }

    return normalizeBlock(minusLines) === normalizeBlock(plusLines);
  } catch {
    return false;
  }
}

// בדיקת דמיון תוכן לזיהוי קובץ ששונה שמו לפני ביצוע commit
function isContentSimilar(textA, textB) {
  const normA = normalizeBlock(textA.split(/\r?\n/));
  const normB = normalizeBlock(textB.split(/\r?\n/));
  if (!normA && !normB) return true;
  if (normA === normB) return true;

  const linesA = new Set(textA.split(/\r?\n/).map(normalizeLine).filter(Boolean));
  const linesB = textB.split(/\r?\n/).map(normalizeLine).filter(Boolean);
  if (linesA.size === 0 || linesB.length === 0) return false;

  let overlap = 0;
  for (const l of linesB) {
    if (linesA.has(l)) overlap++;
  }
  return overlap / Math.max(linesA.size, linesB.length) >= 0.6;
}

function findDailyNote(dateMoment) {
  const dateStr = dateMoment.format("YYYY-MM-DD");

  const dailyOpts = app.internalPlugins?.getPluginById?.('daily-notes')?.instance?.options;
  if (dailyOpts) {
    const folder = (dailyOpts.folder || '').replace(/^\/+|\/+$/g, '');
    const format = dailyOpts.format || 'YYYY-MM-DD';
    const relPath = `${folder ? folder + '/' : ''}${dateMoment.format(format)}.md`;
    const exact = app.vault.getAbstractFileByPath(relPath);
    if (exact) return exact;
  }

  return app.vault.getMarkdownFiles().find(f => f.basename === dateStr) || null;
}

async function syncDailyGit(dateMoment = window.moment(), notify = false, explicitFile = null) {
  if (!vaultPath || !dateMoment?.isValid()) return;

  const dateStr = dateMoment.format("YYYY-MM-DD");
  const file = explicitFile || findDailyNote(dateMoment);
  if (!file) {
    if (notify) new Notice(`לא נמצא פתק יומי עבור ${dateStr}`);
    return;
  }

  const notePath = file.path;
  const watchDir = WATCH_FOLDER.replace(/\/+$/, '');
  const watchPrefix = `${watchDir}/`;
  const isToday = dateStr === window.moment().format("YYYY-MM-DD");
  const gitOpts = { cwd: vaultPath, encoding: 'utf8', timeout: 5000 };

  let baseCommit = EMPTY_TREE;
  let endCommit = EMPTY_TREE;
  try {
    const baseOut = runGit(['rev-list', '-n', '1', '--before', `${dateStr} 00:00:00`, 'HEAD'], gitOpts).trim();
    if (baseOut) baseCommit = baseOut;
    const endOut = runGit(['rev-list', '-n', '1', '--before', `${dateStr} 23:59:59`, 'HEAD'], gitOpts).trim();
    if (endOut) endCommit = endOut;
  } catch {}

  const diffArgs = isToday ? [baseCommit] : [baseCommit, endCommit];
  const candidateFiles = new Map();
  const renamedFromMap = new Map(); // newPath -> oldPath

  // 1. שליפת שינויים מול תחילת היום (כולל שינויי שם שכבר עברו commit)
  try {
    const nameStatus = runGit(['diff', '--name-status', '-M', ...diffArgs, '--', watchDir], gitOpts);
    for (const line of nameStatus.split(/\r?\n/)) {
      if (!line.trim()) continue;
      const parts = line.split('\t');
      const status = parts[0];
      if (status.startsWith('R') && parts[2]) {
        const oldP = parts[1].trim();
        const newP = parts[2].trim();
        candidateFiles.set(newP, 'R');
        renamedFromMap.set(newP, oldP);
      } else if (parts[1]) {
        candidateFiles.set(parts[1].trim(), status[0]);
      }
    }
  } catch {}

  // 2. עבור היום - הוספת קבצים חדשים שעדיין לא נכנסו ל-Git (untracked)
  if (isToday) {
    try {
      const statusOut = runGit(['status', '--porcelain', '-uall', '--', watchDir], gitOpts);
      for (const l of statusOut.split(/\r?\n/)) {
        if (l.startsWith('?? ')) {
          const p = l.slice(3).trim().replace(/^"|"$/g, '');
          candidateFiles.set(p, 'A');
        }
      }
    } catch {}
  }

  // 3. הצלבת קבצים שנמחקו (D) מול קבצים חדשים (A) לזיהוי שינוי שם לפני commit
  const deletedPaths = [...candidateFiles.entries()]
    .filter(([f, st]) => st === 'D' && f.startsWith(watchPrefix) && f.endsWith('.md'))
    .map(([f]) => f);

  const addedPaths = [...candidateFiles.entries()]
    .filter(([f, st]) => st === 'A' && f.startsWith(watchPrefix) && f.endsWith('.md'))
    .map(([f]) => f);

  if (deletedPaths.length > 0 && addedPaths.length > 0 && baseCommit !== EMPTY_TREE) {
    for (const delPath of deletedPaths) {
      let oldContent = '';
      try {
        oldContent = runGit(['show', `${baseCommit}:${delPath}`], gitOpts);
      } catch {
        continue;
      }

      for (const addPath of addedPaths) {
        if (renamedFromMap.has(addPath)) continue;
        try {
          const fullNewPath = path.join(vaultPath, ...addPath.split('/'));
          const newContent = fs.readFileSync(fullNewPath, 'utf8');
          if (isContentSimilar(oldContent, newContent)) {
            candidateFiles.delete(delPath);
            candidateFiles.set(addPath, 'R');
            renamedFromMap.set(addPath, delPath);
            break;
          }
        } catch {}
      }
    }
  }

  // 4. סינון סופי: רק קובצי md בתיקייה, ללא קבצים ששונו רק בגלל עדכון לינקים
  const relevant = [];
  for (const [f, status] of candidateFiles.entries()) {
    if (!f.startsWith(watchPrefix) || !f.endsWith('.md') || f === notePath) continue;
    if (status === 'M' && isLinkOnlyChange(f, diffArgs, gitOpts)) continue;
    relevant.push(f);
  }
  relevant.sort((a, b) => a.localeCompare(b, 'he'));

  await app.vault.process(file, (content) => {
    const escapedHeader = HEADER.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    const regex = new RegExp(`(^${escapedHeader}(?:\\r?\\n|$))([\\s\\S]*?)(?=\\r?\\n## |(?![\\s\\S]))`, 'm');

    // שימור הערות קיימות בטבלה (כולל העברת הערות מקובץ ששונה שמו)
    const savedRows = new Map();
    const match = content.match(regex);
    if (match?.[2]) {
      for (const line of match[2].split(/\r?\n/)) {
        const trimmed = line.trim();
        if (!trimmed.startsWith('|') || /^\|[\s:-]+\|/.test(trimmed)) continue;

        const cells = trimmed
          .replace(/^\||\|$/g, '')
          .replace(/\\\|/g, '§§PIPE§§')
          .split('|')
          .map(c => c.replace(/§§PIPE§§/g, '\\|').trim());

        if (cells[0] === 'קובץ') continue;

        const activeMatch = cells[0]?.match(/^\[\[([^\]\\|]+)(?:\\?\|[^\]]+)?\]\]$/);
        const deletedMatch = cells[0]?.match(/^~~([^~]+)~~\s*\(נמחק\)$/);
        const rowData = { course: cells[1] || '', notes: cells[2] || '' };

        if (activeMatch) {
          const p = activeMatch[1].trim();
          savedRows.set(p, rowData);
          savedRows.set(p.split('/').pop(), rowData);
        } else if (deletedMatch) {
          savedRows.set(deletedMatch[1].trim(), rowData);
        }
      }
    }

    let body;
    if (relevant.length === 0) {
      body = '- לא נערכו קבצים בתיקייה היום';
    } else {
      const tableHeader = '| קובץ | קורס | הערות על השינויים |\n| --- | --- | --- |';
      const rows = relevant.map(f => {
        const noExt = f.replace(/\.md$/, '');
        const name = noExt.split('/').pop();
        const targetFile = app.vault.getAbstractFileByPath(f);

        const oldPathNoExt = renamedFromMap.get(f)?.replace(/\.md$/, '');
        const oldName = oldPathNoExt?.split('/').pop();
        const prev =
          savedRows.get(noExt) ||
          savedRows.get(name) ||
          (oldPathNoExt && savedRows.get(oldPathNoExt)) ||
          (oldName && savedRows.get(oldName)) ||
          { course: '', notes: '' };

        let courseVal = prev.course || '—';
        if (targetFile) {
          const rawCourse = app.metadataCache.getFileCache(targetFile)?.frontmatter?.course;
          const formatted = Array.isArray(rawCourse) ? rawCourse.join(', ') : (rawCourse ?? '');
          courseVal = String(formatted).trim() ? escapePipes(String(formatted).trim()) : '—';
        }

        const fileCell = targetFile ? `[[${noExt}\\|${name}]]` : `~~${name}~~ (נמחק)`;
        const notesCell = prev.notes || '';

        return `| ${fileCell} | ${courseVal} | ${notesCell} |`;
      });

      body = `${tableHeader}\n${rows.join('\n')}`;
    }

    const safeBody = body.replace(/\$/g, '$$$$');
    return regex.test(content)
      ? content.replace(regex, `${HEADER}\n${safeBody}\n`)
      : `${content.trimEnd()}\n\n${HEADER}\n${body}\n`;
  });

  if (notify) {
    new Notice(`טבלת הקבצים עודכנה (${file.path})`);
  }
}

window.syncDailyGit = syncDailyGit;

const CMD_ID = 'daily-git-sync-refresh';
if (app.commands?.commands?.[CMD_ID]) {
  app.commands.removeCommand(CMD_ID);
}
app.commands.addCommand({
  id: CMD_ID,
  name: 'Daily Note: רענון טבלת קבצים מ-Git',
  callback: () => {
    const activeFile = app.workspace.getActiveFile();
    if (activeFile) {
      const parsed = window.moment(activeFile.basename, "YYYY-MM-DD", true);
      if (parsed.isValid()) {
        syncDailyGit(parsed, true, activeFile);
        return;
      }
    }
    syncDailyGit(window.moment(), true);
  }
});

const runSync = () => {
  syncDailyGit(window.moment().subtract(1, 'days'));
  syncDailyGit(window.moment());
};

runSync();
clearInterval(window.dailyGitInterval);
window.dailyGitInterval = setInterval(runSync, 60 * 60 * 1000);
%>