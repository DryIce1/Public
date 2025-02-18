<%*
/**
 * This script:
 * 1. Reads the current file's content from tp.file.content.
 * 2. Checks whether the file starts with a YAML frontmatter (i.e. first line is '---').
 *    - If yes, it finds the closing YAML separator (the second occurrence of '---') and inserts some text after it.
 *    - If not, it inserts that text after the first line.
 * 3. Uses Obsidian's vault.modify function to update the file.
 *
 * Note: Changing a file’s content like this modifies your note permanently.
 */

// Retrieve the current file as a TFile.
const currentFile = tp.file.find_tfile(tp.file.title);

// Read current file content.
let content = tp.file.content;

// The text you wish to insert.
const insertionText = "\n\n```meta-bind-embed\n[[Periodic notes daily note - meta bind embed]]\n```\n";

// Function to insert after the first newline.
function insertAfterFirstLine(text, insertion) {
    return text.replace(/^.*(\r?\n)/, "$&" + insertion);
}

if (/^\s*---\s*$/.test(content.split("\n")[0])) {
    // YAML exists; try to capture the YAML frontmatter.
    const yamlRegex = /^(---[ \t]*\r?\n)([\s\S]*?)(^---[ \t]*\r?\n)/m;
    if (yamlRegex.test(content)) {
        // Replace the match by appending the insertion text after the closing YAML separator.
        content = content.replace(yamlRegex, (match, open, yamlBody, closing) => {
            // closing.trimEnd() removes trailing whitespace/newlines so insertionText gets appended nicely.
            return open + yamlBody + closing.trimEnd() + insertionText;
        });
    } else {
        // Fallback: insert after the first line.
        content = insertAfterFirstLine(content, insertionText);
    }
} else {
    // No YAML frontmatter: insert after the first line.
    content = insertAfterFirstLine(content, insertionText);
}

// Now update the current file using Obsidian's API.
await tp.app.vault.modify(currentFile, content);

%>