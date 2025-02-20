<%*
let selectedText = tp.file.selection();
let replacedText = selectedText.replace(/\[.*?\]/, "[]");
let prefixOptions = ["eTG - ", "RACGP - ", "UpToDate - ", "DermNet - "];
let chosenPrefix = await tp.system.suggester(prefixOptions, prefixOptions);
let fileTitle = tp.file.title;
let finalText = replacedText.replace("[]", `[${chosenPrefix}${fileTitle}]`);
%><% finalText %> %% Date accessed:: (<%tp.date.now("YYYY-MM-DD")%>) %%
