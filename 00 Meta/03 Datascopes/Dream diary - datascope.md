---
aliases: 
banner: https://i.pinimg.com/736x/61/e6/05/61e605c621ed663987c45e220388dbb9.jpg
banner_x: 0.5
banner-y: 30
date-created: 2023-01-01
publish: false
type:
  - datascope
description: a dream log where the daily note contains `dream` or `dreams` metadata
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
```dataview
TABLE 
    WITHOUT ID 
    file.link as Date,
    default(dream, dreams)
FROM
    #log 
WHERE
    dreams OR dream
SORT file.name desc
```
