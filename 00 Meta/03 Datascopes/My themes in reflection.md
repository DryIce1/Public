---
aliases: Monthly themes
banner: https://wallpapercave.com/wp/wp4645793.jpg
cssclass: wide-dataview
date-created: 2023-01-01
publish: false
banner_y: 0.5
type: datascope
---

```dataview
TABLE WITHOUT ID
    file.link as Date,
    theme as Themes
FROM #log AND -"00 Meta"
WHERE theme != null
SORT file.day desc
```
