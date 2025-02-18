<%*
/****************************
 * Script: Select Random Notes from a Folder
 * Improvements:
 * - Prompt user to select a folder.
 * - Handle cases with less than 3 files in the folder.
 * - Ensure unique file selection.
 ****************************/

// Prompt user to select the folder
const folderNames = tp.app.vault.getAllLoadedFiles()
    .filter(x => x instanceof tp.obsidian.TFolder)
    .map(x => x.path);

const selectedFolderPath = await tp.system.suggester(folderNames, folderNames, true);
if (!selectedFolderPath) {
    new Notice("No folder selected. Script terminated.");
    return;
}

// Get the selected folder
const selectedFolder = tp.app.vault.getAbstractFileByPath(selectedFolderPath);
if (!(selectedFolder instanceof tp.obsidian.TFolder)) {
    new Notice("Invalid folder selected. Script terminated.");
    return;
}

// Get all markdown files from the selected folder
const files = tp.app.vault.getMarkdownFiles().filter(file => file.path.startsWith(selectedFolder.path));

// Handle cases where there are less than 3 files
let selectedFiles = [];
if (files.length > 0) {
    // Set the number of random files to pick (up to a maximum of 3 or the number of available files)
    const numFilesToPick = Math.min(files.length, 3);
    
    // Create a set of random files to ensure uniqueness
    const pickedIndexes = new Set();
    while (pickedIndexes.size < numFilesToPick) {
        pickedIndexes.add(Math.floor(Math.random() * files.length));
    }
    
    // Extract selected files using the random indexes
    selectedFiles = [...pickedIndexes].map(index => files[index]);
} else {
    new Notice("No files found in the selected folder. Script terminated.");
    return;
}

// Print the selected files as checkboxes
let output = "";
selectedFiles.forEach(file => {
    output += `- [ ] [[${file.basename}]]\n`;
});
tR += output;
%>
