<%*
const selection = tp.file.selection();
if (selection) {
const replace = selection.split("\n").map(select => {
    if (select.includes("")) {
        select = select.replace(/^((\s+)?(\-|- \[.\]|\d\.?|\d\. \[.\]) )?([a-z])/g, function($4) {return $4.toUpperCase()});
    }
return select;
}).join("\n");
tR += replace;
} else {
    new Notice("Text must be selected");
};
%>