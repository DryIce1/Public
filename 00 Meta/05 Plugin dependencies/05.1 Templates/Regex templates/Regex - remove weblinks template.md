<%*
const content = tp.file.content
const removeweblinks = content.replace(/\[(.*?)\]\(.*?\)/g, "$1")
const file = tp.file.find_tfile(tp.file.title)
await app.vault.modify(file, removeweblinks)
%>