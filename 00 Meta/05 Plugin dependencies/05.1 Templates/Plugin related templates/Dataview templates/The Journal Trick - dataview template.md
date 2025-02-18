```dataview
TABLE 
    WITHOUT ID
    link(Source, dateformat(date(Source), "EEE d MMM yy")) as Date___, 
    rows.Comments as "Journal Trick"
FROM "20 My"
WHERE  contains(cmnt, this.file.name)
FLATTEN cmnt as Comments
WHERE contains(Comments, this.file.name)
GROUP BY file.link as Source
SORT rows.file.day desc
```