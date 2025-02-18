```dataview
TABLE WITHOUT ID 
    file.link as File_________________________________, 
    Done, 
    map(file.etags, (✅) => regexreplace(✅, ".+\/(.+)$", "$1")) as "Tags",
    Priority, 
    do-date as "Do_Date", 
    due-date as "Due_Date", 
    recur-length as "Recur Length",
    project as Project______
from #✅ and !"00 Meta"
where do-date <= date(tomorrow) and done != 1
sort do-date asc, priority
```
