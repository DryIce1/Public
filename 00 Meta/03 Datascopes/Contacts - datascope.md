---
aliases:
  - Catch up with friends and family - datascope
banner: https://i.imgur.com/OoJmZ9r.jpg
banner-y: 40
banner_x: 0.31165
cssclasses: 
date-created: 2023-01-01
publish: false
type:
  - datascope
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
```dataview
TABLE WITHOUT ID
    choice(date(today) - date(reverse(sort(rows.IN))[0]) >= default(Person.contact-frequency, " "), "![call emoji|35](https://i.pinimg.com/474x/db/07/d1/db07d10fbeefc4653b376003717c977f.jpg)", "<center>✓</center>") AS "Call today?",
    Person as Person,
    reverse(sort(rows.IN))[0] as "Last contact",
    date(today) - date(reverse(sort(rows.IN))[0]) AS "Last contact (days)"
FROM -"00 Meta"
FLATTEN file.inlinks as IN
WHERE contains(IN.file.tags, "#log/day")
WHERE contact-frequency
GROUP BY file.link AS Person
SORT reverse(sort(rows.IN))[0] ASC
```

%%
- In order to only show people who are due to contact, add the following: 
WHERE date(today) - date(reverse(sort(rows.IN))[0]) >= default(Person.contact-frequency, " ")
%%