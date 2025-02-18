---
cssclasses:
  - dvl-c
type: datascope
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
```dataview
TABLE
    choice(
        none(default(cover, banner), false), 
        "", 
        "<img align='left' width='120' src='" + default(cover, banner) + "'>") as ""
FROM !"00 Meta"
    where contains(tags, "on/food")
    OR contains(type, "recipes")
SORT 
    file.cday desc, 
    file.name asc
```
