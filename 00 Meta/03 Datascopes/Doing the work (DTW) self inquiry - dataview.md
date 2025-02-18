---
aliases: 
banner: https://i.imgur.com/k7VqUM7.jpg
date-created: 2022-06-01
publish: false
type: datascope
---
> [!NOTE] Doing the work (since the beginning of **`=date(now).year - 1`**)

```dataview
Table WITHOUT Id
    link(
        dateformat(date-created, "yyyy-MM-dd"), 
        dateformat(date-created, "ccc d MMM yyyy")
        ) AS Date,
    file.link AS Reflection,
    summary
FROM 
    !"00 Meta"
WHERE 
    contains(type, "my/reflections/dtw")  
    AND
    (date-created.year >= date(now).year - 1)
SORT date-created desc
```
