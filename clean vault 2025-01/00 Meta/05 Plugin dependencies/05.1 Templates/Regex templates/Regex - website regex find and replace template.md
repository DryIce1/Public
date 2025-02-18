<%*
/****************************
 * This script modifies the current file by applying a series of transformations to clean up and structure the text.
 * - It removes header symbols, adjusts list formatting, replaces double line breaks, creates new lists,
 *   and adjusts list appearance with bold formatting.
 ****************************/
const content = tp.file.content;
    // Remove header symbols ('#') from markdown headers
    const modifyDecreaseHeader = content.replace(/\#\s/g, " ");
    // Indent existing list items by adding four spaces before each item
    const indentExistingLists = modifyDecreaseHeader.replace(/(\s*)-\s+/g, "$1    - ");
    // Replace double line breaks with a single line break to tidy up the text
    const doubleLineBreaks = indentExistingLists.replace(/\n(\s+)?\n/g, "\n");
    // Add list markers ('-') before each new line that starts with alphanumeric characters
    const makeLists = doubleLineBreaks.replace(/\n([a-zA-Z0-9])/g, "\n- $1");
    // Optionally, strip links from the content (commented out, but can be used if needed)
    // const stripLinks = makeLists.replace(/(?<!\!)\[(.+)\]\(([^ ]+?)( "(.+)")?\)/g, "$1");
    // Add list markers ('-') before lines starting with bold text
    const boldLists = makeLists.replace(/\n(\*\*.*\*\*)/g, "\n- $1");
    // Convert italicized text to bold within lists and add list markers ('-')
    const changeItalicToBoldLists = boldLists.replace(/\n_([a-zA-Z0-9_ ]*\:*)_/g, "\n- **$1**");
    // Clean up lists by ensuring there is only one space after each list marker
    const cleanLists = changeItalicToBoldLists.replace(/-\s+/g, "- ");
    // Find the current file and apply the modified content back to it
const file = tp.file.find_tfile(tp.file.title);
await app.vault.modify(file, cleanLists);
%>