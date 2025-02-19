---
aliases:
  - Recently modified
banner: 
banner_y: 1
cssclasses:
  - wide-dataview
date-created: 2023-01-01
publish: true
type:
  - datascope
---
 
```dataview
table 
    dateformat(file.mtime, "ccc d MMM HH:mm") as "Last modified"
from ""
where file.mtime >= date(tomorrow) - dur(2 day)
sort file.mtime desc
limit 100
```
