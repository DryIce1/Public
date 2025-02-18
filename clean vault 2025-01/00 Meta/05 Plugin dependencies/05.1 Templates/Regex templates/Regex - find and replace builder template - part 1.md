<%*
const content = tp.file.content
const <%tp.file.cursor(1)%> = content.replace(/<%tp.file.cursor(2)%>/g, "<%tp.file.cursor(3)%>")
const file = tp.file.find_tfile(tp.file.title)
await app.vault.modify(file, <%tp.file.cursor(1)%>)
%>
