```dataview 
LIST 
WITHOUT ID
"There are " + length(rows.file.name) + " completed books on the shelf." 
FROM 
    #book
    AND !"00 Meta"
WHERE done != null
GROUP BY type
```