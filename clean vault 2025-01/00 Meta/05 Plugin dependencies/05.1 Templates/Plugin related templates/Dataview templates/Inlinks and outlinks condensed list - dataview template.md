```dataview
table WITHOUT ID 
    join(rows.file.link, " | ") as Outgoing
    FROM outgoing([[]])
    SORT file.name
    GROUP BY abc
```
```dataview
table WITHOUT ID 
    join(rows.file.link, " | ") as "Incoming Links"
    FROM [[]]
    SORT file.name
    GROUP BY abc
```
