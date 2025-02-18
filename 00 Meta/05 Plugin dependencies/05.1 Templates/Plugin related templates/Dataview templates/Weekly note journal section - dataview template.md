
```dataview
TABLE without id
    link(date, dateformat(date.file.day, "ccc d MMM")) as date,
    rows.Lists.text as Journal
FROM [[<% tp.file.title %>]] and #log/day 
FLATTEN file.lists AS Lists
WHERE file.day >= date(this.days[0]) 
WHERE file.day <= date(this.days[6])
WHERE meta(Lists.section).subpath = "Log"
GROUP BY file.link as date 
SORT file.day asc
```
