
```dataview
LIST 
join(choice(file.tasks.completed, "●", "○"), "") 
FROM #project AND -"00 Meta"
```