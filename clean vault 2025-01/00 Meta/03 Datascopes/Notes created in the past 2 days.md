---
aliases: Recently created
banner: https://c4.wallpaperflare.com/wallpaper/449/959/627/birth-color-dust-fiction-wallpaper-preview.jpg
banner_y: 1
cssclass: wide-dataview
date-created: 2023-01-01
publish: true
type: datascope
---

```dataview
table dateformat(default(date-created, file.ctime), "ccc d MMM HH:mm") as "date created"
from ""
where default(date-created, file.ctime) >= date(tomorrow) - dur(2 day)
sort file.ctime desc
limit 100
```
