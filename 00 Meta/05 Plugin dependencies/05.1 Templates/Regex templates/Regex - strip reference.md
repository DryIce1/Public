<%*
const content = tp.file.content
const stripreference = content.replace(/\[\d+\]\(.*?\)/g, "")
const file = tp.file.find_tfile(tp.file.title)
await app.vault.modify(file, stripreference)
%>