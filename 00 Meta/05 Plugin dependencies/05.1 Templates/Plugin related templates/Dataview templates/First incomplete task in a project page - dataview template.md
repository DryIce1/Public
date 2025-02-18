```dataview
TABLE Without ID
    rows.file.link[0] AS Page, 
    rows.myTasks.text[0] AS Task
FROM #log/day OR #project
WHERE file.tasks 
FLATTEN file.tasks AS myTasks 
WHERE !myTasks.completed 
GROUP BY file.link
sort rows.file.day desc
limit 5
```
