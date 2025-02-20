---
aliases:
  - Recently modified
banner: https://c4.wallpaperflare.com/wallpaper/449/959/627/birth-color-dust-fiction-wallpaper-preview.jpg
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
