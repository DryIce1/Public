```dataview
TABLE
("![](" + banner +")") as Banner, 
"<i>" + description + "</i>" AS "Description"
WHERE file.folder = this.file.folder
WHERE file.name != this.file.name
SORT file.mtime DESC
```
<%* 
// Rename file to folder (folder note)
const folderPath = tp.file.folder(true) 
const folder = folderPath.split("/")[1] + ' (folder note)'
await tp.file.rename(folder) 
%>
