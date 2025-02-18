---
up:
  - "[[Meta bind embeds (MOC, datascope)]]"
date-created: 2024-11-11
cssclasses: 
tags:
  - meta-bind-embed
---
```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
> [!NOTE]- # All mentions of *`=default(this.aliases[0], this.file.name)`*
> 
> ```dataview
> TABLE 
>     WITHOUT ID
>      choice(
>         file.day, 
>         dateFormatDay, 
>         dateFormatDateCreated + "<sub><small>" + "<br>    *" + 
>             link(file.link, truncate(string(file.name), 24)) + "*</sub></small>" 
>         ) AS "date (name)",
>     list.text AS "mentioned"
> FROM [[]] 
>     OR #log/day
>     AND -"00 Meta"
> WHERE any(file.lists, (list) => contains(lower(list.text), lower(this.file.name)))
>     OR any(file.lists, (list) => contains(lower(list.text), string(lower(this.aliases[0]))))
> FLATTEN file.lists AS list
> WHERE 
>     contains(lower(list.text), lower(this.file.name))
>     OR contains(lower(list.text), string(lower(this.aliases[0])))
> FLATTEN link(file.link, dateformat(file.day, "EEE, d MMM") + " '" + dateformat(file.day, "yy")) AS dateFormatDay
> FLATTEN link(dateformat(default(file.day, default(date-created, file.cday)), "EEE, d MMM")
>     + " '" + dateformat(default(file.day, default(date-created, file.cday)), "yy")) AS dateFormatDateCreated
> SORT default(file.day, default(date-created, file.cday)) DESC
> ```

