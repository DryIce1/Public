
```dataview 
LIST 
WITHOUT ID
"There are " + length(rows.file.name) + " completed books on the shelf." 
FROM 
 !"00 Meta"
 WHERE contains(type, "book") AND contains(status, "done")
GROUP BY type
```
