<%*
/****************************
 * Script: Move, Copy, or Embed Text Between Notes
 * Version: Optimized to fix asynchronous behavior issues with trailing newlines for embeds.
 * Features:
 * - Move, copy, or embed content from the current note to another note.
 * - User prompts for target note, move mode, and content placement.
 * - Enhanced with block reference capabilities.
 ****************************/

/****************************
 * SCRIPT STARTS BELOW *
 ****************************/

// Main function that performs the move/copy/embed action
async function main() {
    // Prompt user for move mode
    const moveModeMessage = "Select move mode: 'Move' to move the text, 'Copy' to duplicate, 'Embed' to move and link, or 'BlockRef' to create a block reference and link.";
    const moveModes = ["move", "copy", "embed", "blockRef"];
    const moveModeLabels = ["Move (default)", "Copy (duplicate)", "Embed (move and link)", "BlockRef (create block ref and link)"];
    const moveMode = await tp.system.suggester(moveModeLabels, moveModes, false, moveModeMessage) || "move";

    // Prompt user to select the file to move content to
    const targetFileMessage = "Select the target note to move, copy, or embed the content to:";
    const targetFile = await tp.system.suggester(
        (file) => file.path,
        this.app.vault.getMarkdownFiles(),
        false,
        targetFileMessage
    );

    if (!targetFile) {
        new Notice("No target file selected. Script terminated.");
        return;
    }

    // Ask user if they want to open the target file after moving the content
    const openAfterMoveMessage = "Do you want to open the target file after the move operation?";
    const openFileOptions = ["Yes", "No"];
    const openAfterMoveResponse = await tp.system.suggester(openFileOptions, [true, false], false, openAfterMoveMessage);
    const openAfterMove = openAfterMoveResponse === true;

    // Get selected text or current line
    const cmEditor = this.app.workspace.activeLeaf.view.editor;
    if (tp.file.selection() === '') {
        const currentLine = cmEditor.getCursor().line;
        cmEditor.setSelection({ line: currentLine, ch: 0 }, { line: currentLine, ch: 9999 });
    }
    let selectedText = tp.file.selection();
    let resultText = selectedText;

    // Handle embedding or block references
    if (["blockRef", "embed"].includes(moveMode)) {
        const blockId = createBlockHash();
        if (moveMode === "blockRef") {
            // Add block reference to original note and create link in target note
            resultText = `${selectedText} ^${blockId}`;
            selectedText = `![[${tp.file.title}#^${blockId}]]\n`.replace(/\n/g, "");
        } else {
            // Embed selected text into target note with a block reference
            resultText = `![[${targetFile.basename}#^${blockId}]]\n`.replace(/\n/g, "");
            selectedText = `\n${selectedText} ^${blockId}\n`;
        }
        navigator.clipboard.writeText(resultText).then(() => {}); // Copy link to clipboard
    }

    // Read the current content of the target file
    const targetContent = await this.app.vault.read(targetFile);
    let updatedContent = await placeContentByUserSelection(targetContent, selectedText);

    // Update the target file with the new content
    await this.app.vault.modify(targetFile, updatedContent);

    // Remove selected content if the mode is 'move'
    if (moveMode === "move") {
        cmEditor.replaceSelection(''); // Delete from original file
    }

    // Optionally open the target file
    if (openAfterMove) {
        await this.app.workspace.openLinkText(targetFile.basename, targetFile.path, true);
    }

    tR = resultText;
}

// Function to create a unique block hash for block references
function createBlockHash() {
    let result = '';
    const characters = 'abcdefghijklmnopqrstuvwxyz0123456789';
    const length = 7;
    for (let i = 0; i < length; i++) {
        result += characters.charAt(Math.floor(Math.random() * characters.length));
    }
    return result;
}

// Function to place content under a specific line selected by the user
async function placeContentByUserSelection(currentContent, contentToInsert) {
    const contentLines = currentContent.split('\n');
    const lineOptions = ['--End of File--', '--Beginning of File--'];
    const lineValues = ['last_', 'first_'];

    contentLines.forEach((line) => {
        if (line.trim() !== '') {
            lineOptions.push(line.slice(0, 250));
            lineValues.push(line);
        }
    });

    const lineSelectionMessage = "Select the line after which the content should be inserted:";
    const selectedLine = await tp.system.suggester(lineOptions, lineValues, false, lineSelectionMessage);

    if (selectedLine === 'last_') {
        return `${currentContent}\n${contentToInsert}`;
    } else if (selectedLine === 'first_') {
        return `${contentToInsert}\n${currentContent}`;
    } else if (currentContent.includes(`\n${selectedLine}\n`)) {
        return currentContent.replace(`\n${selectedLine}\n`, `\n${selectedLine}\n${contentToInsert}\n`);
    } else if (currentContent.startsWith(`${selectedLine}\n`)) {
        return currentContent.replace(`${selectedLine}\n`, `${selectedLine}\n${contentToInsert}\n`);
    } else {
        return `${currentContent}\n${contentToInsert}`;
    }
}

// Execute the main function
await main();
%>