---
type:
course: <% (function() { let f = tp.file.find_tfile('Session Variable'); if(f) { let c = app.metadataCache.getFileCache(f); if(c?.frontmatter?.course) return c.frontmatter.course; } return 'Unassigned'; })() %>
chapter: "[[ ]]"
tags:
created: <% tp.date.now("Do MMMM YYYY") %>
---
## <% tp.file.title %>