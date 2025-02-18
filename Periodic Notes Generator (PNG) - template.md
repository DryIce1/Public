<%*
/****************************
 # Periodic Notes Generator
 * Version: 1.6
 * Automates the creation of periodic notes with logging and error handling.
 * The PNG generates periodic notes (yearly, quarterly, monthly, and weekly). 
     * For weekly notes, it uses ISO week numbering standards. ISO weeks start on **Monday** and ends on**Sunday**. This means that  weeks at the beginning or end of a year might belong to the previous or next year, which can sometimes be counterintuitive. But the PNG and periodic notes templates will ensure that there is no overlapping or skipping because they are all referring to the same ISO standards.
 ****************************/

/****************************
 * CONFIGURATION SECTION
 ****************************/
const TEMPLATE_NAME = "Periodic Notes Generator";
const VERSION = "1.6";
const LOG_PREFIX = "[PNG]";
console.log(`${LOG_PREFIX} ${TEMPLATE_NAME} - Version: ${VERSION} initialized.`);

const BASE_FOLDER = "20 My/21 Journal"; // Base folder path for notes
const ADD_BASE_YEAR = true; // Whether to include year-based subfolders

// Paths to template files
const TEMPLATES = {
    yearly: "00 Meta/06 Plugin dependencies/06.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - yearly note.md",
    quarterly: "00 Meta/06 Plugin dependencies/06.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - quarterly note.md",
    monthly: "00 Meta/06 Plugin dependencies/06.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - monthly note.md",
    weekly: "00 Meta/06 Plugin dependencies/06.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - weekly note.md",
};

// Configuration options
const INCLUDE_OVERLAPPING_WEEKS = false; // Whether to include weeks that overlap from previous or next months

let createdFiles = [];
let skippedFiles = [];

function logProgress(code, message) {
    console.log(`${LOG_PREFIX} [${code}] ${message}`);
}

function logError(code, message, error) {
    console.error(`${LOG_PREFIX} [${code}] ${message}`, error);
}

(async () => {
    try {
        // Step PN-2001: Validate the current date
        logProgress('PN-2001', 'Validating current date.');
        const currentDate = moment(tp.file.title, "YYYY-MM-DD");
        if (!currentDate.isValid()) {
            new Notice(`${TEMPLATE_NAME}: Invalid date format in file title. Expected YYYY-MM-DD.`, 10000);
            logError('PN-4001', 'Invalid date format in file title.', new Error('Invalid date'));
            return;
        }
        if (currentDate.date() !== 1) {
            new Notice(`${TEMPLATE_NAME}: No notes created. Not the first day of the month.`, 10000);
            logProgress('PN-2002', 'Exited. Not the first day of the month.');
            return;
        }
        logProgress('PN-2003', `Current date validated: ${currentDate.format('YYYY-MM-DD')}`);

        // Step PN-2004: Setup base folder
        logProgress('PN-2004', 'Setting up base folder.');
        let baseFolderPath = tp.obsidian.normalizePath(BASE_FOLDER);
        if (ADD_BASE_YEAR) {
            baseFolderPath = tp.obsidian.normalizePath(`${BASE_FOLDER}/${currentDate.format("YYYY")}`);
            if (!(await app.vault.adapter.exists(baseFolderPath))) {
                await app.vault.createFolder(baseFolderPath);
                logProgress('PN-2005', `Created folder: ${baseFolderPath}`);
            } else {
                logProgress('PN-2006', `Base folder exists: ${baseFolderPath}`);
            }
        }

        // Step PN-2007: Validate templates
        logProgress('PN-2007', 'Validating templates.');
        for (const [type, path] of Object.entries(TEMPLATES)) {
            const templatePath = tp.obsidian.normalizePath(path);
            if (!(await app.vault.adapter.exists(templatePath))) {
                throw new Error(`Template missing for ${type} notes: ${templatePath}`);
            }
            TEMPLATES[type] = templatePath; // Update to normalized path
        }
        logProgress('PN-2008', 'All templates validated.');

        // Step PN-2009: Generate periodic notes
        logProgress('PN-2009', 'Generating periodic notes.');
        const fileNames = {
            yearly: `${baseFolderPath}/${currentDate.format("YYYY")}.md`,
            quarterly: `${baseFolderPath}/${currentDate.format("YYYY")}-Q${currentDate.quarter()}.md`,
            monthly: `${baseFolderPath}/${currentDate.format("YYYY-MM")}.md`,
        };

        for (const [type, filePath] of Object.entries(fileNames)) {
            const result = await createNote(type, TEMPLATES[type], filePath);
            if (result.status === "created") createdFiles.push(filePath);
            else skippedFiles.push(filePath);
        }

        const weeklyNotes = await generateWeeklyNotes(currentDate);
        createdFiles.push(...weeklyNotes.filter(note => note.status === "created").map(note => note.path));
        skippedFiles.push(...weeklyNotes.filter(note => note.status === "skipped").map(note => note.path));

        // Step PN-2010: Summarize results
        new Notice(`Periodic Notes Summary:\nCreated: ${createdFiles.length}\nSkipped: ${skippedFiles.length}`, 10000);
        logProgress('PN-2010', `Created files: ${JSON.stringify(createdFiles)}`);
        logProgress('PN-2011', `Skipped files: ${JSON.stringify(skippedFiles)}`);
    } catch (error) {
        logError('PN-4002', 'Error encountered during execution.', error);
        new Notice(`${TEMPLATE_NAME}: Error encountered. Check console for details.`, 10000);
    }
})();

/****************************
 * HELPER FUNCTIONS
 ****************************/
async function createNote(noteType, templatePath, targetPath) {
    try {
        targetPath = tp.obsidian.normalizePath(targetPath);
        if (await app.vault.adapter.exists(targetPath)) {
            logProgress(`PN-3001`, `Skipped creating ${noteType} note. File exists: ${targetPath}`);
            return { status: "skipped", path: targetPath };
        }
        const templateFile = tp.file.find_tfile(templatePath);
        if (!templateFile) {
            throw new Error(`Template file not found: ${templatePath}`);
        }
        const content = await app.vault.read(templateFile);
        const newFile = await app.vault.create(targetPath, content);
        logProgress(`PN-200${noteType.charAt(0).toUpperCase()}`, `Created ${noteType} note: ${newFile.path}`);
        return { status: "created", path: newFile.path };
    } catch (error) {
        logError(`PN-400${noteType.charAt(0).toUpperCase()}`, `Error creating ${noteType} note.`, error);
        return { status: "error", path: targetPath };
    }
}

async function generateWeeklyNotes(currentDate) {
    const startOfMonth = currentDate.clone().startOf("month");
    const endOfMonth = currentDate.clone().endOf("month");
    const weeklyNotes = [];
    let weekStart = startOfMonth.clone().startOf('isoWeek');

    while (weekStart.isBefore(endOfMonth.clone().add(1, 'day'))) {
        // Include weeks that start in the current month
        if (weekStart.month() !== currentDate.month()) {
            weekStart.add(1, 'week');
            continue;
        }
        const weekYear = weekStart.isoWeekYear();
        const weekNumber = weekStart.isoWeek();
        const weekFolderPath = tp.obsidian.normalizePath(
            ADD_BASE_YEAR ? `${BASE_FOLDER}/${weekYear}` : BASE_FOLDER
        );

        // Ensure the week year folder exists
        if (!(await app.vault.adapter.exists(weekFolderPath))) {
            await app.vault.createFolder(weekFolderPath);
            logProgress('PN-2005', `Created folder for week year: ${weekFolderPath}`);
        }

        const weekPath = `${weekFolderPath}/${weekYear}-W${weekNumber.toString().padStart(2, '0')}.md`;
        const result = await createNote("weekly", TEMPLATES.weekly, weekPath);
        weeklyNotes.push(result);
        weekStart.add(1, "week");
    }

    return weeklyNotes;
}
%>