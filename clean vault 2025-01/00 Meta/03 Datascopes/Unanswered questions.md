---
aliases: 
banner: https://i.pinimg.com/736x/61/e6/05/61e605c621ed663987c45e220388dbb9.jpg
banner_x: 0.5
banner-y: 30
date-created: 2023-01-01
publish: false
type:
  - datascope
description: find again unanswered questions in the vault `- [?] `
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
```dataview
TABLE
    WITHOUT ID
    "<no-styling>" + rows.file.link + "</no-styling>" AS File,
    "- [?] <strong>" + filter(rows.T, (t) => !t.completed).text + "</strong>" AS "Questions"
FROM !"00 Meta"
WHERE contains(file.tasks.status, "?")
FLATTEN file.tasks AS T
GROUP BY file.link
SORT rows.file.mtime DESC
```
