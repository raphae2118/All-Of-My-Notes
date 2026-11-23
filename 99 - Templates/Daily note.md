---
date: <% tp.file.title %>
tags:
  - daily
---

[[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>|אתמול]] | [[<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>|מחר]]

# <% tp.date.now("DD.MM.YYYY", 0, tp.file.title, "YYYY-MM-DD") %>

## c
- 

## מחשבות / סיכום קצר


## פתוח להמשך
- [ ] 

## קבצים שנערכו היום
<%* setTimeout(() => window.syncDailyGit?.(window.moment(tp.file.title, "YYYY-MM-DD")), 400); %>