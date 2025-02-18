<%*
/****************************
 # Periodic Notes Generator
 * Version: 1.8.1
 * Automates the creation of yearly, quarterly, monthly, weekly, and daily notes with logging and error handling.
 * 
 * This script creates notes for:
 *   • Yearly:   YYYY.md
 *   • Quarterly: YYYY-Q[1..4].md
 *   • Monthly:   YYYY-MM.md
 *   • Weekly:    YYYY-W##.md (ISO 8601 weeks)
 *   • Daily:     YYYY-MM-DD.md (for all days in the current month)
 *
 * ISO weeks start on Monday and end on Sunday. The template will ensure it generates all weekly notes relevant for the current month's days,
 * even if the week contains days in the preceding or following month.
 ****************************/

/****************************
 * CONFIGURATION SECTION
 ****************************/
const TEMPLATE_NAME = "Periodic Notes Generator";
const VERSION = "1.8.1";
const LOG_PREFIX = "[PNG]";
console.log(`${LOG_PREFIX} ${TEMPLATE_NAME} - Version: ${VERSION} initialized.`);

// Base folder path for all notes
const BASE_FOLDER = "20 My/21 Journal";

// Whether to create subfolders per year, e.g., "20 My/21 Journal/2026"
const ADD_BASE_YEAR = true;

// Paths to template files
const TEMPLATES = {
    yearly:    "00 Meta/05 Plugin dependencies/05.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - yearly note.md",
    quarterly: "00 Meta/05 Plugin dependencies/05.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - quarterly note.md",
    monthly:   "00 Meta/05 Plugin dependencies/05.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - monthly note.md",
    weekly:    "00 Meta/05 Plugin dependencies/05.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - weekly note.md",
    daily:     "00 Meta/05 Plugin dependencies/05.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes template - daily note.md"
};

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
        // Step PN-2001: Validate the current date from the file title.
        logProgress('PN-2001', 'Validating current date.');
        const currentDate = moment(tp.file.title, "YYYY-MM-DD");
        if (!currentDate.isValid()) {
            new Notice(`${TEMPLATE_NAME}: Invalid date format in file title. Expected YYYY-MM-DD.`, 10000);
            logError('PN-4001', 'Invalid date format in file title.', new Error('Invalid date'));
            return;
        }

        // Only create periodic notes on the first day of the month
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

        // Step PN-2007: Validate existence of template files
        logProgress('PN-2007', 'Validating templates.');
        for (const [type, path] of Object.entries(TEMPLATES)) {
            const templatePath = tp.obsidian.normalizePath(path);
            if (!(await app.vault.adapter.exists(templatePath))) {
                throw new Error(`Template missing for ${type} notes: ${templatePath}`);
            }
            TEMPLATES[type] = templatePath; // Store normalized path
        }
        logProgress('PN-2008', 'All templates validated.');

        // Step PN-2009: Generate yearly, quarterly, and monthly notes
        logProgress('PN-2009', 'Generating yearly, quarterly, and monthly notes.');
        const fileNames = {
            yearly:    `${baseFolderPath}/${currentDate.format("YYYY")}.md`,
            quarterly: `${baseFolderPath}/${currentDate.format("YYYY")}-Q${currentDate.quarter()}.md`,
            monthly:   `${baseFolderPath}/${currentDate.format("YYYY-MM")}.md`,
        };

        for (const [type, filePath] of Object.entries(fileNames)) {
            const result = await createNote(type, TEMPLATES[type], filePath);
            if (result.status === "created") {
                createdFiles.push(filePath);
            } else {
                skippedFiles.push(filePath);
            }
        }

        // Step PN-2010: Generate weekly notes
        logProgress('PN-2010', 'Generating ISO weekly notes.');
        const weeklyNotes = await generateWeeklyNotes(currentDate, baseFolderPath);
        createdFiles.push(...weeklyNotes.filter(note => note.status === "created").map(note => note.path));
        skippedFiles.push(...weeklyNotes.filter(note => note.status === "skipped").map(note => note.path));

        // Step PN-2014: Generate daily notes for the current month
        logProgress('PN-2014', 'Generating daily notes.');
        const dailyResult = await generateDailyNotes(currentDate, baseFolderPath);

        // Step PN-2011: Summarize results in a detailed notice
        displaySummaryNotice(createdFiles, skippedFiles, {
            created: dailyResult.dailyCreatedCount,
            skipped: dailyResult.dailySkippedCount
        });

        // Also log the created & skipped lists
        logProgress('PN-2012', `Created files: ${JSON.stringify(createdFiles)}`);
        logProgress('PN-2013', `Skipped files: ${JSON.stringify(skippedFiles)}`);

    } catch (error) {
        logError('PN-4002', 'Error encountered during execution.', error);
        new Notice(`${TEMPLATE_NAME}: Error encountered. Check console for details.`, 10000);
    }
})();

/****************************
 * HELPER FUNCTIONS
 ****************************/

/**
 * Creates a periodic note from the designated template if it doesn't already exist.
 * @param {string} noteType - "yearly" | "quarterly" | "monthly" | "weekly" | "daily"
 * @param {string} templatePath - The normalized path to the note template file.
 * @param {string} targetPath - The normalized path where the new note should be created.
 * @returns {{status: string, path: string}} An object containing either "created", "skipped", or "error".
 */
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

/****************************
 * WEEKLY NOTES GENERATION
 ****************************/

/**
 * Generates ISO 8601 weekly notes for the month containing currentDate.
 * @param {moment.Moment} currentDate - The date object from the file's title.
 * @param {string} baseFolderPath - The base folder path for periodic notes.
 * @returns {Array<{status: string, path: string}>} A list of results for each weekly note creation.
 */
async function generateWeeklyNotes(currentDate, baseFolderPath) {
    // For the given month, find the Monday of the first ISO week that intersects this month,
    // and the Sunday of the last ISO week that intersects this month.
    const startOfMonthISO = currentDate.clone().startOf("month").startOf("isoWeek");
    const endOfMonthISO = currentDate.clone().endOf("month").endOf("isoWeek");

    let weekStart = startOfMonthISO.clone();
    const weeklyNotes = [];

    while (weekStart.isSameOrBefore(endOfMonthISO, "day")) {
        const weekYear = weekStart.isoWeekYear();
        const weekNumber = weekStart.isoWeek();

        // Ensure the correct folder path for the weekly note's year
        const weekFolderPath = tp.obsidian.normalizePath(
            ADD_BASE_YEAR ? `${BASE_FOLDER}/${weekYear}` : BASE_FOLDER
        );
        if (!(await app.vault.adapter.exists(weekFolderPath))) {
            await app.vault.createFolder(weekFolderPath);
            logProgress('PN-2005', `Created folder for week year: ${weekFolderPath}`);
        }

        const weekPath = `${weekFolderPath}/${weekYear}-W${weekNumber.toString().padStart(2, '0')}.md`;
        const result = await createNote("weekly", TEMPLATES.weekly, weekPath);
        weeklyNotes.push(result);

        // Move ahead by one ISO week
        weekStart.add(1, "week");
    }

    return weeklyNotes;
}

/****************************
 * DAILY NOTES GENERATION
 ****************************/

/**
 * Generates daily notes for every day in the month containing currentDate.
 * @param {moment.Moment} currentDate - The date object from the file's title.
 * @param {string} baseFolderPath - The base folder path for periodic notes.
 * @returns {{dailyCreatedCount: number, dailySkippedCount: number}} Counts of created and skipped daily notes.
 */
async function generateDailyNotes(currentDate, baseFolderPath) {
    const startOfMonth = currentDate.clone().startOf("month");
    const endOfMonth = currentDate.clone().endOf("month");
    let day = startOfMonth.clone();
    let dailyCreatedCount = 0;
    let dailySkippedCount = 0;

    while (day.isSameOrBefore(endOfMonth, "day")) {
        // Filename format: YYYY-MM-DD.md
        const dailyFilePath = `${baseFolderPath}/${day.format("YYYY-MM-DD")}.md`;
        const result = await createNote("daily", TEMPLATES.daily, dailyFilePath);
        if (result.status === "created") {
            dailyCreatedCount++;
        } else {
            dailySkippedCount++;
        }
        day.add(1, "day");
    }
    return { dailyCreatedCount, dailySkippedCount };
}

/****************************
 * SUMMARY NOTICE
 ****************************/

/**
 * Generates a detailed notice summarizing which notes were created and skipped.
 * For daily notes, only a summary count is shown.
 * @param {Array<string>} createdFiles - List of non-daily note file paths that were created.
 * @param {Array<string>} skippedFiles - List of non-daily note file paths that were skipped.
 * @param {{created: number, skipped: number}} [dailySummary] - Optional summary for daily notes.
 */
function displaySummaryNotice(createdFiles, skippedFiles, dailySummary) {
    // Helper to extract the base name (no folder, no extension)
    function getFileName(path) {
        return path.split("/").pop().replace(/\.md$/, "");
    }

    const createdNames = createdFiles.map(path => `- ${getFileName(path)}`).join("\n");
    const skippedNames = skippedFiles.map(path => `- ${getFileName(path)}`).join("\n");

    let summaryMessage =
`Periodic Notes Summary

Files already existing:
${skippedNames || "- None"}

Created files:
${createdNames || "- None"}
`;

    if (dailySummary) {
        summaryMessage += `

Daily notes:
- ${dailySummary.created} daily notes created
- ${dailySummary.skipped} daily notes skipped
`;
    }

    new Notice(summaryMessage, 10000);
}
%>