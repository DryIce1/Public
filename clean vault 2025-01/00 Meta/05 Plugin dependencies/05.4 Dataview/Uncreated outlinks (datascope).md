---
up: 
related: 
aliases: 
date-created: 2024-12-08
banner: 
banner-y: 
cover: 
tags: 
publish: false
type:
  - datascope
area: 
summary:
---
# Datascope


```dataview
TABLE  without id
    out AS "Uncreated files", 
    file.link as "Origin"
FROM "20 My/22 Reflections"
FLATTEN file.outlinks as out
WHERE !(out.file) AND !contains(meta(out).path, "/")
SORT out ASC
LIMIT 150
```



