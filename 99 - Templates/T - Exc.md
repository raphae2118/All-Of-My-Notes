---
type: Exercise
course: <% (function() { let f = tp.file.find_tfile('Session Variable'); if(f) { let c = app.metadataCache.getFileCache(f); if(c?.frontmatter?.course) return c.frontmatter.course; } return 'Unassigned'; })() %>
chapter: <% (function() { let f = tp.file.find_tfile('Session Variable'); if(f) { let c = app.metadataCache.getFileCache(f); if(c?.frontmatter?.chapter) return c.frontmatter.chapter; } return 'Unassigned'; })() %>
difficulty:
created: <% tp.date.now("Do MMMM YYYY") %>
---

## <% tp.file.title %>

## השאלה

$$
$$

## פתרון
1. 


### מאיפה זה בא
### לאן זה הולך


