---
up: 
related: 
aliases:
  - Highlights, lessons learned, future plans
  - Periodic note insights
date-created: 2024-12-06
banner: 
banner-y: 
cover: 
publish: false
type:
  - datascope
area: 
summary: 
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
# Journal insights

> [!NOTE] adjust the date timeframe and file limit

- **date-from**:: 2020-01-01
- **date-to**:: 2025-01-17
- **limitBind**:: 200

```dataview
TABLE WITHOUT ID
file.link + " " + replace(filter(file.etags, (x) => startswith(x, "#log")), "#log/day", "") as date, 
choice(nonnull(highlights), "highlights : " + default(highlights + "<br>", ""), null)
    + choice(nonnull(wins), "wins : " + default(wins + "<br>", ""), null)
    + choice(nonnull(lessons-learned), "lessons-learned : " + default(lessons-learned + "<br>", ""), null)
    + choice(nonnull(disappointments), "disappointments : " + default(disappointments + "<br>", ""), null)
    + choice(nonnull(working-towards), "working-towards : " + default(working-towards + "", ""), null)
    + choice(nonnull(next-plans), "next-plans : " + default(next-plans + "", ""), null)
    as "journal insights"
FROM #log
WHERE file.cday <= date(tomorrow)
WHERE 
    highlights !=null
    OR wins != null
    OR lessons-learned !=null
    OR disappointments !=null
    OR working-towards !=null
    OR future-plans !=null
LIMIT this.limitBind
SORT date-created desc, file.name desc
```
WHERE date-created >= date(this.date-from)  and date-created <= date(this.date-to) 


