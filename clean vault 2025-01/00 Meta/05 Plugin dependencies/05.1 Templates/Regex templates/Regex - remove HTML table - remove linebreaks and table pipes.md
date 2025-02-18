<%*
let selectedText = tp.file.selection();
let firstLineBullet = selectedText.replace(/\^(\.)/, "- $1");
let replaceNoteNo = selectedText.replace(/Note \d+:/gm, "\n- ");
let replacePipes = replaceNoteNo.replace(/\|\s*/gm, "");
let finalText = replacePipes.replace(/\<br\>/gm, "");
let fileTitle = tp.file.title;
%><% finalText %>