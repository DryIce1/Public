---
aliases: 
banner: https://i.pinimg.com/originals/4a/73/18/4a73186742620ddc29db89c955f14e09.jpg
date-created: 2023-02-18
publish: true
tags:
  - atlas/guidance
cssclasses:
  - dvl-c
banner-y: 70
---

```dataview
TABLE
("![](" + banner +")") as Banner, 
"<i>" + description + "</i>" AS "Description"
WHERE file.folder = this.file.folder
WHERE file.name != this.file.name
SORT file.mtime DESC
```
