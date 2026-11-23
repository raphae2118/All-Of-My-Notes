---
type: Law
course: <% (function() { let f = tp.file.find_tfile('Session Variable'); if(f) { let c = app.metadataCache.getFileCache(f); if(c?.frontmatter?.course) return c.frontmatter.course; } return 'Unassigned'; })() %>
chapter: <% (function() { let f = tp.file.find_tfile('Session Variable'); if(f) { let c = app.metadataCache.getFileCache(f); if(c?.frontmatter?.chapter) return c.frontmatter.chapter; } return 'Unassigned'; })() %>
tags:
created: <% tp.date.now("Do MMMM YYYY") %>
---

## <% tp.file.title %>

### נוסח החוק

### קשור אל
### מאיפה זה בא
### לאן זה הולך


