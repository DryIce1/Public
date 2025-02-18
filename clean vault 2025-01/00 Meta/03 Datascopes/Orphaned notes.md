---
aliases: 
banner: https://i.imgur.com/sAnpIzk.jpg
cssclass: wide-dataview
date-created: 2023-02-06
publish: false
type: datascope
---
> [!NOTE] Notes without any links appear here
> - Only files without tags: `AND !file.tags` 

```dataview 
TABLE
    file.cday as "Date created",
    file.mday as "Date modified"
FROM ""
    AND !#emancipated
    AND !"00 Meta"
WHERE !file.inlinks and !file.outlinks 
    AND file.name != [[]].file.name 
SORT file.name DESC
```
