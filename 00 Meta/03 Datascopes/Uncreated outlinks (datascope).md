---
up: 
related: 
aliases: 
date-created: 2024-12-08
banner: 
banner-y: 
cover: 
publish: false
type:
  - datascope
area: 
summary: 
---
# Datascope
- select search path
- select an appropriate limit
```dataview
TABLE  without id
    out AS "Uncreated files", 
    file.link as "Origin"
FROM ""
FLATTEN file.outlinks as out
WHERE !(out.file) AND !contains(meta(out).path, "/")
SORT out DESC
LIMIT 20
```



