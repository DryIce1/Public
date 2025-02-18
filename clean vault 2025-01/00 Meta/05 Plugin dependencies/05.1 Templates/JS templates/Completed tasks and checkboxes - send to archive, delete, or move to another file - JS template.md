<%*
// Step 1: Prompt user to choose action - Archive, Delete, or Move
const options = ["Archive Completed Tasks", "Delete Completed Tasks", "Move Completed Tasks to Another File"];
const userChoice = await tp.system.suggester(options, options);

// Step 2: Read the content of the current file
const currentFile = await app.vault.read(app.workspace.getActiveFile());

// Step 3: Split the content into lines
let lines = currentFile.split('\n');

// Step 4: Prepare lists for current content and completed tasks
let updatedContent = [];
let completedTasks = [];

// Step 5: Iterate over each line and categorize as completed or keep it
for (let line of lines) {
    // Check if the line is a completed task
    if (line.trim().startsWith("- [x]")) {
        // Move to the list of completed tasks
        completedTasks.push(line);
    } else {
        // Keep all other lines in the updated content
        updatedContent.push(line);
    }
}

// Step 6: Process user's choice
if (userChoice === "Archive Completed Tasks") {
    // Step 6.1: Check if there is an '# Archived tasks' section in the content
    let archivedTasksHeaderIndex = updatedContent.findIndex(line => line.trim() === "# Archived tasks");

    if (archivedTasksHeaderIndex === -1) {
        // If the '# archived tasks' header doesn't exist, add it to the bottom
        updatedContent.push("\n# archived tasks\n");
        archivedTasksHeaderIndex = updatedContent.length - 1; // Update the index to the new header
    }

    // Step 6.2: Insert the completed tasks under the '# archived tasks' section
    if (completedTasks.length > 0) {
        updatedContent.splice(archivedTasksHeaderIndex + 1, 0, ...completedTasks);
    }
} else if (userChoice === "Delete Completed Tasks") {
    // Step 6.3: If user chooses to delete, simply do not add the completed tasks to updatedContent
    // In this case, completedTasks are not re-added, which effectively deletes them.
} else if (userChoice === "Move Completed Tasks to Another File") {
    // Step 6.4: If the user chooses to move the completed tasks, prompt to select the file

    // Step 6.4.1: Get all files in the vault
    const allFiles = app.vault.getMarkdownFiles();
    const fileNames = allFiles.map(file => file.path);

    // Step 6.4.2: Prompt the user to choose a file to move completed tasks to
    const chosenFilePath = await tp.system.suggester(fileNames, fileNames);
    if (chosenFilePath) {
        // Step 6.4.3: Read the content of the chosen file
        let chosenFileContent = await app.vault.adapter.read(chosenFilePath);

        // Step 6.4.4: Add the completed tasks to the chosen file
        chosenFileContent += `\n# Archived Tasks from ${tp.file.title}\n` + completedTasks.join('\n');

        // Step 6.4.5: Write the updated content to the chosen file
        await app.vault.adapter.write(chosenFilePath, chosenFileContent);
    }
}

// Step 7: Write the updated content back to the current file
await app.vault.modify(app.workspace.getActiveFile(), updatedContent.join('\n'));
%>