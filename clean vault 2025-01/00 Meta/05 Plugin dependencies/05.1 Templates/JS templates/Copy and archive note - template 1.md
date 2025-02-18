<%*
/****************************
 * Copy and Archive Note Template
 * Purpose : Creates an archive copy (with standardized date-based version control) of the current note and moves the new archive note to a designated archive folder.
 ****************************/

/****************************
 * USER CONFIGURATION SECTION
 ****************************/
// User-defined archive base folder
const archiveBaseFolder = "00 Meta/09 Archive"; // Set the base folder path for archives

// Date-based subfolder settings
const addYearSubfolder = true; // Set to true to add a year-based subfolder (default)

// Standardized date suffix format for version control
const dateSuffixFormat = "YYYY-MM-DD-HHmm";

/****************************
 * SCRIPT STARTS BELOW
 ****************************/

// Step 0: Initializing log
console.log("Step 0: Initializing Copy and Archive Note Template...");

// Step 1: Prompt the user to choose an archive action
console.log("Step 1: Prompting user to select an archive action");
const archiveActions = ["Send a copy to archive (add archived date)", "Move file to archive (add archived date)", "Move file to archive (don't add archive date)"];
const selectedAction = await tp.system.suggester(archiveActions, archiveActions, false, "Select an archive action:");
if (!selectedAction) {
    $1
new Notice('An error occurred: ' + error.message, 10000);
    throw new Error("Action selection was canceled.");
}
console.log(`Step 1: User selected action: ${selectedAction}`);

// Step 2: Handle the optional second prompt if user selects "Send a copy to archive (add archived date)"
let openArchivedFile = false;
if (selectedAction === "Send a copy to archive (add archived date)") {
    console.log("Step 2: Prompting user to select whether to open the archived file");
    const copyOptions = ["Don't Open Archived File", "Open Archived File in New Tab"];
    const selectedCopyOption = await tp.system.suggester(copyOptions, copyOptions, false, "Choose an option for the archived file:");
    if (!selectedCopyOption) {
        $1
new Notice('An error occurred: ' + error.message, 10000);
        throw new Error("Copy option selection was canceled.");
    }
    openArchivedFile = selectedCopyOption === "Open Archived File in New Tab";
    console.log(`Step 2: User selected copy option: ${selectedCopyOption}`);
}

// Step 3: Find the current active file and copy its content to avoid issues with Templater code
console.log("Step 3: Finding the current active file and reading its content");
const currentFile = tp.file.find_tfile(tp.file.title);
if (!currentFile) {
    $1
new Notice('An error occurred: ' + error.message, 10000);
    throw new Error("Unable to find the current file.");
}
const originalContent = await app.vault.read(currentFile);
const originalTitle = tp.file.title;
const currentFilePath = await tp.file.path(true);
console.log(`Step 3: Current file details - Title: ${originalTitle}, Path: ${currentFilePath}, Content: ${originalContent}`);

// Step 4: Define the new title for the archived copy with version control
console.log("Step 4: Defining the new title for the archived copy");
const dateSuffix = tp.date.now(dateSuffixFormat);
let newTitle = `${originalTitle} (archive ${dateSuffix})`;
console.log(`Step 4: New title defined: ${newTitle}`);

// Step 5: Handle duplicate file names by incrementing the suffix
console.log("Step 5: Checking for duplicate file names");
let duplicateCount = 1;
let targetFilePath = `${archiveBaseFolder}/${newTitle}.md`;
while (await tp.file.exists(targetFilePath)) {
    console.warn(`Step 5 [Warning]: File "${newTitle}" already exists in "${archiveBaseFolder}", incrementing suffix.`);
    newTitle = `${originalTitle} (archive ${dateSuffix})_${duplicateCount}`;
    targetFilePath = `${archiveBaseFolder}/${newTitle}.md`;
    duplicateCount++;
}
console.log(`Step 5: Final new title after duplicate check: ${newTitle}`);

// Step 6: Define the archive folder path based on user settings
console.log("Step 6: Defining the archive folder path");
const currentYear = tp.date.now("YYYY");
let targetFolder = archiveBaseFolder;
if (addYearSubfolder) {
    targetFolder += `/${currentYear}`;
}
console.log(`Step 6: Target folder path defined: ${targetFolder}`);

// Ensure the target folder exists, create it if it doesn't
try {
    const targetFolderTFolder = app.vault.getAbstractFileByPath(targetFolder);
    if (!targetFolderTFolder) {
        console.log("Step 6: Target folder does not exist, creating folder");
        await app.vault.createFolder(targetFolder);
        console.log("Step 6: Target folder created successfully");
    }
} catch (error) {
    $1
new Notice('An error occurred: ' + error.message, 10000);
    throw new Error(`Failed to create target folder "${targetFolder}".`);
}

// Step 7: Create a copy of the current file (or move it if "Move file to archive (add archived date)" is selected)
console.log("Step 7: Creating or moving the file based on the selected action");
let newFilePath;
try {
    if (selectedAction === "Send a copy to archive (add archived date)") {
        console.log(`Step 7: Copying the file to the archive folder at path: ${targetFolder}`);
        newFilePath = `${targetFolder}/${newTitle}.md`;
        await app.vault.create(newFilePath, originalContent);
        console.log(`Step 7: File copied successfully - Title: ${newTitle}, Path: ${newFilePath}, Content: ${originalContent}`);
    } else if (selectedAction === "Move file to archive (add archived date)") {
        console.log("Step 7: Moving the file to the archive folder");
        newFilePath = `${targetFolder}/${newTitle}`;
        await tp.file.move(newFilePath, currentFile);
        console.log(`Step 7: File moved successfully - Title: ${newTitle}, Path: ${newFilePath}`);
    } else if (selectedAction === "Move file to archive (don't add archive date)") {
        console.log("Step 7: Moving the file to the archive folder without renaming");
        newFilePath = `${targetFolder}/${originalTitle}`;
        await tp.file.move(newFilePath, currentFile);
        console.log(`Step 7: File moved successfully without renaming - Title: ${originalTitle}, Path: ${newFilePath}`);
    }
} catch (error) {
    $1
new Notice('An error occurred: ' + error.message, 10000);
    throw new Error("Failed to create or move the file.");
}

// Step 8: Open the archived file if requested
if (openArchivedFile) {
    console.log("Step 8: Opening the archived file in a new tab");
    try {
        await app.workspace.openLinkText(newTitle, targetFolder, true);
        console.log(`Step 8: Archived file opened successfully - Title: ${newTitle}, Path: ${targetFolder}/${newTitle}.md`);
    } catch (error) {
        $1
new Notice('An error occurred: ' + error.message, 10000);
    }
}

// Step 9: Notify the user that the archive operation is complete
console.log("Step 9: Notifying the user that the archive operation is complete");
new Notice(`File successfully archived to ${targetFolder}`, 10000);
console.log("Step 9: Archive operation completed successfully");
%>