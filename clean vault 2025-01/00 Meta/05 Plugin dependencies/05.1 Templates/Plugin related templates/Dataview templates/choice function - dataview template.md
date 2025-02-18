```dataview
TABLE 
    choice(done, "✅", "❌") as Done, 
    dateformat(Due, "yyyy-MM-dd") as Due,
    Priority
FROM
    #task
```
