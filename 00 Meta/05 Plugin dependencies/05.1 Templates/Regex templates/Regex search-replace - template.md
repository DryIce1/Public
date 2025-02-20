<%*
// Locate the current file:
const file = tp.file.find_tfile(tp.file.path(true));

// Retrieve the current file content using tp.file.content:
const content = tp.file.content;

// Define the regex pattern for the target text.
// Note: special characters are escaped so the match is exact.
const searchPattern = /Regex search pattern/g;

// Define the replacement text (identical to the search text):
const replacementText = 'Regex replace pattern';

// Perform the replacement:
const newContent = content.replace(searchPattern, replacementText);

// Update the file using Obsidian's API. Even if no change is detected, re-writing the file triggers a refresh.
await app.vault.modify(file, newContent);

tR ;
%>

