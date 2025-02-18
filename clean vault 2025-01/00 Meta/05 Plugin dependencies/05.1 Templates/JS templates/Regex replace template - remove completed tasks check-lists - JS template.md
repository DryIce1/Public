<%*
const content = tp.file.content
const updatedContent = content.replace(/- \[x\] .*\n/g, "")
await app.vault.modify(tp.file, updatedContent)
%>