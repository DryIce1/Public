---
aliases: 
banner: https://i.imgur.com/kf1JbDk.png
cssclasses:
  - wide-dataview
date-created: 2022-11-28
publish: true
type:
  - datascope
---
# Most linked notes


```dataview
TABLE
    "↗" + length(file.outlinks) + ", ↙" + length(file.inlinks) + ", (" + sum(length(file.outlinks) + length(file.inlinks)) +")"  AS "↗Outgoing links, ↙Backlinks, (total)"
FROM !"00 Meta" 
    AND !"20 My/21 Journal"
WHERE
    !contains(file.name, "example exlusion") 
SORT sum(length(file.outlinks) + length(file.inlinks)) desc
LIMIT 20
```

# Most linked [[Maps of content]]
```dataview
TABLE
    "↗" + length(file.outlinks) + ", ↙" + length(file.inlinks) + ", (" + sum(length(file.outlinks) + length(file.inlinks)) +")"  AS "↗Outgoing links, ↙Backlinks, (total)"
FROM #my/MOC AND #MOC AND !"00 Meta"
WHERE
    !contains(file.name, "example exlusion") AND
    !contains(file.name, "example exlusion 2")
SORT sum(length(file.outlinks) + length(file.inlinks)) desc
LIMIT 20
```
