---
aliases: 
date-created: 2023-08-18
publish: false
status: 
priority: 
type:
  - datascope
cssclasses:
  - dvl-c
  - wide-dataview
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
```dataview
table
     "![](" + banner + ")" as banner
FROM #log/day
where banner != null
sort date(file.name) desc
limit 50
```
