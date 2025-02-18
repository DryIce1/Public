---
aliases: 
banner: https://i.imgur.com/OoJmZ9r.jpg
banner-y: 40
cssclasses: 
date-created: 2023-01-01
description: upcoming birthdays
publish: false
type:
  - datascope
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
```dataview
TABLE WITHOUT ID
    file.link AS Person,
    dateformat(birthdate, "DD") as "Birthday",
    round((choice(!dod, date(today), dod) - birthdate).years) + " years old" as Age, 
    (choice(!dod, date(today), dod) - birthdate).days + " days" AS "Days Alive"
FROM -"00 Meta"
WHERE contains(type, "person") WHERE birthdate
SORT birthdate.month asc
```
%% WHERE !dod %%

> [!NOTE]- # Birthdays this month
> ```dataview
> TABLE WITHOUT ID
>     file.link AS Person,
>     dateformat(birthdate, "DD") as "Birthday",
>     round((choice(!dod, date(today), dod) - birthdate).years) + " years old" as Age, 
>     (choice(!dod, date(today), dod) - birthdate).days + " days" AS "Days Alive"
> FROM -"00 Meta"
> WHERE contains(type, "person") 
> WHERE contains(type, "person") WHERE birthdate
> WHERE date(birthdate).month = date(today).month
> SORT birthdate.month asc
> ```
> 

> [!NOTE]- # Birthdays today
> ```dataview
> TABLE WITHOUT ID
>     file.link AS Person,
>     dateformat(birthdate, "DD") as "Birthday",
>     round((choice(!dod, date(today), dod) - birthdate).years) + " years old" as Age, 
>     (choice(!dod, date(today), dod) - birthdate).days + " days" AS "Days Alive"
> FROM -"00 Meta"
> WHERE contains(type, "person") 
> WHERE contains(type, "person") WHERE birthdate
> WHERE dateformat(birthdate, "MM-dd") = dateformat(date(today), "MM-dd")
> SORT birthdate.month asc
> ```
> 

> [!NOTE]- # Dead people's birthdays
> ```dataview
> TABLE WITHOUT ID
>     file.link AS Person,
>     dateformat(birthdate, "DD") as "Birthday",
>     round((choice(!dod, date(today), dod) - birthdate).years) + " years old" as Age, 
>     (choice(!dod, date(today), dod) - birthdate).days + " days" AS "Days Alive"
> FROM -"00 Meta"
> WHERE contains(type, "person") WHERE birthdate
> WHERE dod
> SORT birthdate.month asc
> ```

