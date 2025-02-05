<%*
/*******************************************************************************
 * GLOBAL NOTE TEMPLATE (GNT)
 * Template for creating and managing different note types in Obsidian
 * Author: <Your Name or Team>

 ******************************************************************************/

/******************************************************************************
 * DEBUG ON/OFF SETTING
 * Set to "true" to enable debug & warn logs; "false" to only show errors.
 ******************************************************************************/
const DEBUG_ENABLED = false;

/******************************************************************************
 * CONFIGURATION
 ******************************************************************************/
const TEMPLATE_NAME  = "Global Note Template";
const VERSION        = "7";
logStep("INIT_1", `${TEMPLATE_NAME} v${VERSION} initialized.`, "debug");

/** Default settings applied if not explicitly overridden. */
const DEFAULT_TYPE_SETTINGS = {
  path: "/",
  template: null,
  publishMode: "prompt",
  renameRule: null
};

/**
 * Configuration for all recognized note types.
 * Each property can define:
 *  - path          (folder path)
 *  - template      (path to a note template)
 *  - publishMode   ("always", "never", or "prompt")
 *  - renameRule    (e.g. { rule: "prefix"|"suffix", value: "..." })
 *  - customMetadata (object with frontmatter key/value pairs)
 *  - subtypes      (array or object for sub-categories)
 */
const TYPES_CONFIG = {
  inbox: {
    type: "note"
  },

  note: {
    path: "30 Notes"
  },

  medicine: {
    type: "medical_note",
    path: "30 Notes/31 Medicine",
    template: "00 Meta/06 Plugin dependencies/06.1 Templates/Note structure templates/Templates relating to my workflow/Medicine - diagnosis.md",
    publishMode: "always"
  },

  my: {
    type: "note",
    area: "my",
    path: "20 My/22 Reflections",
    publishMode: "never"
  },

  recipe: {
    type: "recipe",
    path: "30 Notes",
    template: "00 Meta/06 Plugin dependencies/06.1 Templates/Note structure templates/Recipe and food - template.md",
    customMetadata: {
      up: "[[Recipes (MOC)]]",
      ingredients: null,
      "prep-time": null,
      cuisine: null
    },
    renameRule: { rule: "suffix", value: " - recipe" },
    publishMode: "always"
  },

  project: {
    type: "project",
    subtypes: ["project/personal", "project/work", "project/community"],
    path: "40 Projects",
    template: "00 Meta/06 Plugin dependencies/06.1 Templates/Note structure templates/Project note - template.md",
    customMetadata: {
      quarter: "\"[[" + await tp.date.now("YYYY-[Q]Q") + "]]\"",
      deadline: null,
      priority: null
    },
    publishMode: "prompt"
  },

  source: {
    type: "source",
    path: "10 Sources",
    publishMode: "always",
    subtypes: {
      book: {
        template: "00 Meta/06 Plugin dependencies/06.1 Templates/Note structure templates/Book review template.md",
        renameRule: { rule: "suffix", value: " - Book" }
      },
      article: { type: "article" },
      website: { type: "website" },
      LLM: { type: "LLM" },
      podcast: { type: "podcast" },
      podcast_episode: { type: "podcast_episode" },
      video: { type: "video" },
      movie: { renameRule: { rule: "prefix", value: "🎥 " } },
      series: { type: "series" },
      author: { type: "author" }
    },
    customMetadata: {
      url: null,
      source: null,
      status: null,
      rating: null
    }
  },

  person: {
    type: "person",
    path: "20 My/24 People",
    template: "00 Meta/06 Plugin dependencies/06.1 Templates/Note structure templates/Person note - template.md",
    customMetadata: {
      birthdate: null,
      "contact-frequency": null
    },
    publishMode: "never"
  },

  pack_lists: {
    path: "50 Pack Lists",
    publishMode: "prompt"
  }
};

/** Standard metadata merged into final frontmatter. */
const STANDARD_METADATA = {
  up: null,
  related: null,
  aliases: null,
  "date-created": await tp.date.now("YYYY-MM-DD"),
  banner: null,
  "banner-y": null,
  cover: null,
  tags: null,
  publish: false,
  type: null,
  area: null,
  summary: null
};

/** Keys that are formatted as YAML arrays. */
const listKeys = [
  "up",
  "related",
  "aliases",
  "tags",
  "type",
  "area",
  "quarter",
  "cuisine"
];

/** Secondary templates are searched here. */
const templateDirectoryPath = "00 Meta/06 Plugin dependencies/06.1 Templates/";

/** Where to insert the type template (if used). */
const TEMPLATE_INSERT_LOCATION_AFTER_FRONTMATTER = true;

/** If true, automatically insert the type template without prompting. */
const AUTO_INSERT_TYPE_TEMPLATE = false;

/** Archiving function defaults. */
const ARCHIVE_SETTINGS = {
  baseFolder: "00 Meta/09 Archive",
  addYearSubfolder: true,
  dateSuffixFormat: "YYYY-MM-DD-HHmm"
};

/******************************************************************************
 * LOGGING & DEBUG SETTINGS
 ******************************************************************************/
function logStep(stepId, message, level = "log") {
  // Always show errors. Suppress debug/warn/log if debug is disabled.
  if (!DEBUG_ENABLED && level !== "error") return;

  const out = `[GNT-${stepId}] ${message}`;
  switch (level) {
    case "debug": console.debug(out); break;
    case "warn":  console.warn(out);  break;
    case "error": console.error(out); break;
    default:      console.log(out);   break;
  }
}

/******************************************************************************
 * YAML BUILDING
 ******************************************************************************/
function buildYamlString(metadata) {
  const lines = ["---"];
  for (const [key, value] of Object.entries(metadata)) {
    if (value === null || value === undefined) {
      lines.push(`${key}:`);
    } else if (Array.isArray(value)) {
      lines.push(`${key}:`);
      for (const item of value) {
        lines.push(`  - ${item}`);
      }
    } else {
      lines.push(`${key}: ${value}`);
    }
  }
  lines.push("---");
  return lines.join("\n");
}

/******************************************************************************
 * ACTION SELECTION (APPLY OR CREATE)
 ******************************************************************************/
async function selectAction() {
  try {
    logStep("SEL-ACT_1", "Prompting user for action (Apply or Create)...", "debug");
    const actionOptions = ["Apply to current note", "Create new note"];
    const choice = await tp.system.suggester(
      actionOptions,
      actionOptions,
      false,
      "Select the desired action:"
    );
    if (!choice) throw new Error("Action selection canceled by user");
    logStep("SEL-ACT_2", `Action chosen: ${choice}`, "debug");
    return choice === "Create new note" ? "new" : "current";
  } catch (err) {
    logStep("SEL-ACT_3", `Error selecting action: ${err.message}`, "error");
    throw err;
  }
}

/******************************************************************************
 * TYPE CONFIG RESOLUTION
 ******************************************************************************/
function getTypeConfig(typeKey) {
  const userConfig = TYPES_CONFIG[typeKey];
  if (!userConfig) {
    logStep("TYPECFG_1", `No config found for "${typeKey}". Using defaults.`, "warn");
    const ephemeral = { ...DEFAULT_TYPE_SETTINGS };
    ephemeral.type = typeKey;
    return ephemeral;
  }
  const merged = { ...DEFAULT_TYPE_SETTINGS, ...userConfig };
  if (!("type" in userConfig)) merged.type = typeKey;
  return merged;
}

function mergeParentChildConfigs(parentConfig, subtypeName) {
  if (!parentConfig.subtypes) return parentConfig;
  // Array-based subtypes
  if (Array.isArray(parentConfig.subtypes)) {
    if (!parentConfig.subtypes.includes(subtypeName)) return parentConfig;
    const merged = { ...parentConfig };
    merged.type = subtypeName;
    return merged;
  }
  // Object-based subtypes
  const childConfig = parentConfig.subtypes[subtypeName];
  if (!childConfig) return parentConfig;
  const merged = { ...parentConfig, ...childConfig };
  if (childConfig.customMetadata && parentConfig.customMetadata) {
    merged.customMetadata = { ...parentConfig.customMetadata, ...childConfig.customMetadata };
  }
  if (!("type" in childConfig)) merged.type = subtypeName;
  return merged;
}

/******************************************************************************
 * NAMING CONVENTION
 ******************************************************************************/
function applyNamingConvention(typeConfig, fileName) {
  if (!typeConfig.renameRule) return fileName;
  const { rule, value } = typeConfig.renameRule;
  if (rule === "prefix" && !fileName.startsWith(value)) {
    return value + fileName;
  }
  if (rule === "suffix" && !fileName.endsWith(value)) {
    return fileName + value;
  }
  return fileName;
}

/******************************************************************************
 * PUBLISH MODE HANDLER
 ******************************************************************************/
async function determinePublishStatus(typeConfig) {
  const mode = typeConfig.publishMode || "prompt";
  if (mode === "always") return true;
  if (mode === "never") return false;
  logStep("PUBLISH_1", "Prompting user for publish status...", "debug");
  const publishOptions = ["Publish", "Don't publish"];
  const choice = await tp.system.suggester(
    publishOptions,
    publishOptions,
    false,
    "Publish this note?"
  );
  if (!choice) {
    logStep("PUBLISH_2", "User canceled publish prompt; defaulting false.", "debug");
    return false;
  }
  const result = (choice === "Publish");
  logStep("PUBLISH_3", `User selected publish = ${result}`, "debug");
  return result;
}

/******************************************************************************
 * AREA CHOICE IF area IS AN ARRAY
 ******************************************************************************/
async function promptForAreaChoice(areaArray, typeKey) {
  try {
    logStep("AREA_1", `Prompting user for area of "${typeKey}"...`, "debug");
    const selectedArea = await tp.system.suggester(
      areaArray,
      areaArray,
      false,
      `Select an area for "${typeKey}":`
    );
    return selectedArea;
  } catch (err) {
    logStep("AREA_2", `Error prompting for area: ${err.message}`, "error");
    return null;
  }
}

/******************************************************************************
 * CHOOSE TYPE + (OPTIONAL) SUBTYPE
 ******************************************************************************/
async function pickTypeAndMaybeSubtype() {
  try {
    logStep("TYPESEL_1", "Prompting user for top-level note type...", "debug");
    const topLevelTypes = Object.keys(TYPES_CONFIG).concat(["Other Functions"]);
    const pickedTopLevel = await tp.system.suggester(
      topLevelTypes,
      topLevelTypes,
      false,
      "Select note type (or 'Other Functions')"
    );
    if (!pickedTopLevel) {
      const customType = await tp.system.prompt("No selection. Enter a custom type name:");
      if (!customType) throw new Error("Type selection canceled by user.");
      return { finalType: customType, isCustom: true };
    }
    if (pickedTopLevel === "Other Functions") {
      logStep("TYPESEL_2", "User selected Other Functions.", "debug");
      await handleOtherFunctions();
      return { finalType: null, isCustom: false };
    }
    // Load the parent config
    const parentConfig = getTypeConfig(pickedTopLevel);
    // If subtypes is array
    if (Array.isArray(parentConfig.subtypes)) {
      if (parentConfig.subtypes.length === 0) {
        return { finalType: pickedTopLevel, isCustom: false, topLevelOnly: true };
      }
      const subPick = await tp.system.suggester(
        parentConfig.subtypes,
        parentConfig.subtypes,
        false,
        `Select a subtype for "${pickedTopLevel}":`
      );
      if (!subPick) {
        return { finalType: pickedTopLevel, isCustom: false, topLevelOnly: true };
      }
      return { finalType: pickedTopLevel, subtype: subPick, isCustom: false };
    }
    // If subtypes is object
    else if (typeof parentConfig.subtypes === "object") {
      const subKeys = Object.keys(parentConfig.subtypes);
      if (subKeys.length === 0) {
        return { finalType: pickedTopLevel, isCustom: false, topLevelOnly: true };
      }
      const chosenSubtype = await tp.system.suggester(
        subKeys,
        subKeys,
        false,
        `Select a subtype for "${pickedTopLevel}":`
      );
      if (!chosenSubtype) {
        return { finalType: pickedTopLevel, isCustom: false, topLevelOnly: true };
      }
      return { finalType: pickedTopLevel, subtype: chosenSubtype, isCustom: false };
    }
    // No subtypes
    return { finalType: pickedTopLevel, isCustom: false, topLevelOnly: true };
  } catch (err) {
    logStep("TYPESEL_6", `Error picking type/subtype: ${err.message}`, "error");
    throw err;
  }
}

/******************************************************************************
 * FILE CREATION / MOVE
 ******************************************************************************/
async function handleFileOperations(typeConfig, action, fileName) {
  try {
    logStep("FILEOPS_1", `File handling. Path: "${typeConfig.path}"`, "debug");
    const folderPath = typeConfig.path ?? "/";
    let targetFilePath;
    let finalTFile = null;

    if (action === "new") {
      const finalName = applyNamingConvention(typeConfig, fileName);
      targetFilePath = `${folderPath}/${finalName}`;
      if (await tp.file.exists(targetFilePath)) {
        throw new Error("Destination file already exists!");
      }
      await tp.file.create_new("", finalName, true, folderPath);
      logStep("FILEOPS_3", `Created new file: ${targetFilePath}`, "debug");
      finalTFile = tp.file.find_tfile(targetFilePath);

    } else {
      // action === "current"
      const currentFilePath = await tp.file.path(true);
      logStep("FILEOPS_4", `Current file path: ${currentFilePath}`, "debug");
      const oldTitle = tp.file.title;
      const renamed = applyNamingConvention(typeConfig, oldTitle);
      targetFilePath = `${folderPath}/${renamed}`;
      if (await tp.file.exists(targetFilePath)) {
        throw new Error("Destination file already exists!");
      }
      await tp.file.move(targetFilePath);
      logStep("FILEOPS_5", `Moved current file to: ${targetFilePath}`, "debug");
      finalTFile = tp.file.find_tfile(targetFilePath);
    }

    // Focus new or moved file
    if (finalTFile) {
      await app.workspace.activeLeaf.openFile(finalTFile);
      const editor = app.workspace.activeLeaf.view?.sourceMode?.cmEditor;
      if (editor) editor.focus();
    }
    logStep("FILEOPS_6", "File handling complete.", "debug");
    return finalTFile;

  } catch (err) {
    if (err.message.includes("already exists")) {
      new Notice("Destination file already exists! \n\nPlease rename or pick another location.", 5000);
    }
    logStep("FILEOPS_7", `File operations failed: ${err.message}`, "error");
    throw err;
  }
}

/******************************************************************************
 * READ TEMPLATE FILE HELPER
 ******************************************************************************/
async function maybeReadFileContent(path) {
  try {
    const f = app.vault.getAbstractFileByPath(path);
    if (!f) {
      logStep("READTPL_1", `Template file not found at "${path}".`, "warn");
      return null;
    }
    return await app.vault.read(f);
  } catch (err) {
    logStep("READTPL_2", `Error reading file content of ${path}: ${err.message}`, "error");
    return null;
  }
}

/******************************************************************************
 * APPLY FRONTMATTER & BODY CHANGES
 ******************************************************************************/
async function applyAllNoteChanges(
  typeConfig,
  publishStatus,
  secondaryTemplateContent,
  targetFile
) {
  try {
    logStep("APPLY_1", "Single-pass write for note...", "debug");
    let file = targetFile;
    if (!file) {
      file = tp.file.find_tfile(tp.file.path(true));
      if (!file) throw new Error("Could not find a file to update frontmatter.");
    }

    const existingContent = await app.vault.read(file);
    const yamlRegex = /^---\s*\n([\s\S]*?)\n---\s*\n?/;
    let existingFrontmatter = {};
    let existingBody = existingContent;
    const match = yamlRegex.exec(existingContent);

    if (match) {
      await app.fileManager.processFrontMatter(file, fm => {
        existingFrontmatter = { ...fm };
      });
      existingBody = existingBody.replace(yamlRegex, "").trim();
    }

    // Merge standard with existing
    let metadataToAdd = { ...STANDARD_METADATA, ...existingFrontmatter };

    // Merge type-specific customMetadata
    if (typeConfig.customMetadata) {
      metadataToAdd = { ...metadataToAdd, ...typeConfig.customMetadata };
      logStep("APPLY_2", "Merged custom metadata from typeConfig.", "debug");
    }

    metadataToAdd.publish = publishStatus;
    if (typeConfig.type)  metadataToAdd.type  = typeConfig.type;
    if (typeConfig.area)  metadataToAdd.area  = typeConfig.area;

    // Convert some keys to arrays
    listKeys.forEach(key => {
      if (metadataToAdd.hasOwnProperty(key) && metadataToAdd[key] !== null) {
        if (!Array.isArray(metadataToAdd[key])) {
          metadataToAdd[key] = [metadataToAdd[key]];
          logStep("APPLY_3", `Converted frontmatter key "${key}" to list.`, "debug");
        }
      }
    });

    // Possibly read type template if not auto
    let typeTemplateContent = null;
    if (typeConfig.template && !AUTO_INSERT_TYPE_TEMPLATE) {
      const insertOptions = ["yes", "no"];
      const userIns = await tp.system.suggester(
        insertOptions,
        insertOptions,
        false,
        "Insert template for this type?"
      );
      if (userIns === "yes") {
        typeTemplateContent = await maybeReadFileContent(typeConfig.template);
      }
    } else if (typeConfig.template && AUTO_INSERT_TYPE_TEMPLATE) {
      typeTemplateContent = await maybeReadFileContent(typeConfig.template);
    }

    // Build final body
    let finalBody = existingBody.trim();
    if (secondaryTemplateContent) {
      finalBody += `\n\n${secondaryTemplateContent.trim()}`;
      logStep("APPLY_4", "Appended secondary template content.", "debug");
    }
    if (typeTemplateContent) {
      if (TEMPLATE_INSERT_LOCATION_AFTER_FRONTMATTER) {
        finalBody = `${typeTemplateContent.trim()}\n\n${finalBody}`;
        logStep("APPLY_5", "Inserted type template after frontmatter.", "debug");
      } else {
        finalBody += `\n\n${typeTemplateContent.trim()}`;
        logStep("APPLY_5", "Appended type template at end.", "debug");
      }
    }

    // Reassemble final frontmatter + body
    const finalYaml    = buildYamlString(metadataToAdd);
    const finalContent = `${finalYaml}\n\n${finalBody}`.trim();
    await app.vault.modify(file, finalContent);
    logStep("APPLY_6", "Wrote updated frontmatter & body in one pass.", "debug");

  } catch (err) {
    logStep("APPLY_7", `Failed to apply note changes: ${err.message}`, "error");
    throw err;
  }
}

/******************************************************************************
 * "OTHER FUNCTIONS" HANDLER
 ******************************************************************************/
async function handleOtherFunctions() {
  try {
    logStep("OTHER_1", "Prompting advanced actions...", "debug");
    const options = [
      "Include a Secondary Template",
      "Archive files",
      "Create a New Template"
    ];
    const selectedOption = await tp.system.suggester(
      options,
      options,
      false,
      "Select an advanced action:"
    );
    if (!selectedOption) {
      logStep("OTHER_2", "User canceled advanced options.", "debug");
      return;
    }
    switch (selectedOption) {
      case "Include a Secondary Template":
        logStep("OTHER_3", "User wants a secondary template.", "debug");
        const secondaryContent = await addSecondaryTemplate();
        if (secondaryContent) {
          globalSecondaryTemplateContent = secondaryContent;
          logStep("OTHER_4", "Stored secondary template content.", "debug");
          await mainNoteCreationFlow();
        } else {
          logStep("OTHER_5", "No secondary template selected.", "debug");
          await mainNoteCreationFlow();
        }
        break;
      case "Archive files":
        logStep("OTHER_6", "User selected archive flow.", "debug");
        await handleArchiveFlow();
        break;
      case "Create a New Template":
        logStep("OTHER_7", "User selected template creation flow.", "debug");
        await createNewTemplate();
        break;
    }
  } catch (error) {
    logStep("OTHER_8", `Advanced option error: ${error.message}`, "error");
    throw error;
  }
}

/******************************************************************************
 * ADD SECONDARY TEMPLATE
 ******************************************************************************/
async function addSecondaryTemplate() {
  try {
    logStep("SEC-TPL_1", "Prompting for a secondary template...", "debug");
    const templates = app.vault.getMarkdownFiles().filter(
      f => f.path.startsWith(templateDirectoryPath)
    );
    if (templates.length === 0) {
      logStep("SEC-TPL_2", `No .md templates found in ${templateDirectoryPath}.`, "warn");
      return null;
    }
    const selectedPath = await tp.system.suggester(
      templates.map(t => t.basename),
      templates.map(t => t.path),
      false,
      "Select a secondary template to include:"
    );
    if (!selectedPath) return null;

    const file = app.vault.getAbstractFileByPath(selectedPath);
    if (!file) {
      logStep("SEC-TPL_3", "File not found after selection.", "warn");
      return null;
    }
    const content = await app.vault.read(file);
    logStep("SEC-TPL_4", `Read secondary template from ${selectedPath}`, "debug");
    return content;
  } catch (err) {
    logStep("SEC-TPL_5", `Error in addSecondaryTemplate: ${err.message}`, "error");
    return null;
  }
}

/******************************************************************************
 * ARCHIVE FLOW
 ******************************************************************************/
async function handleArchiveFlow() {
  try {
    logStep("ARCH_1", "Starting archive flow...", "debug");
    const { baseFolder, addYearSubfolder, dateSuffixFormat } = ARCHIVE_SETTINGS;

    const archiveActions = [
      { code: "COPY_ADD_DATE", display: "Send a copy to archive (+date)" },
      { code: "MOVE_ADD_DATE", display: "Move file to archive (+date)" },
      { code: "MOVE_NO_DATE",  display: "Move file to archive (no date)" }
    ];
    const actionCode = await tp.system.suggester(
      archiveActions.map(a => a.display),
      archiveActions.map(a => a.code),
      false,
      "Select an archive action:"
    );
    if (!actionCode) {
      new Notice("Archive action canceled.", 5000);
      return;
    }
    logStep("ARCH_2", `Archive action code: ${actionCode}`, "debug");

    let openArchivedFile = false;
    if (actionCode === "COPY_ADD_DATE") {
      const copyOpts = [
        { code: "NO_OPEN",  display: "Don't Open Archived File" },
        { code: "OPEN_TAB", display: "Open Archived File in New Tab" }
      ];
      const copyChoice = await tp.system.suggester(
        copyOpts.map(c => c.display),
        copyOpts.map(c => c.code),
        false,
        "Choose an option for archived copy:"
      );
      if (copyChoice === "OPEN_TAB") {
        openArchivedFile = true;
      }
      logStep("ARCH_3", `Open archived file after copy? ${openArchivedFile}`, "debug");
    }

    const currentFile = tp.file.find_tfile(tp.file.title);
    if (!currentFile) {
      new Notice("No current file found.", 4000);
      return;
    }
    const originalContent = await app.vault.read(currentFile);
    const originalTitle   = tp.file.title;
    const dateSuffix      = tp.date.now(dateSuffixFormat);
    let newTitle          = `${originalTitle} (archive ${dateSuffix})`;

    let targetFilePath = `${baseFolder}/${newTitle}.md`;
    let dupCount = 1;
    while (await tp.file.exists(targetFilePath)) {
      newTitle       = `${originalTitle} (archive ${dateSuffix})_${dupCount}`;
      targetFilePath = `${baseFolder}/${newTitle}.md`;
      dupCount++;
    }

    let targetFolder = baseFolder;
    if (addYearSubfolder) {
      const currentYear = tp.date.now("YYYY");
      targetFolder += `/${currentYear}`;
    }
    if (!app.vault.getAbstractFileByPath(targetFolder)) {
      await app.vault.createFolder(targetFolder);
      logStep("ARCH_4", `Created folder: ${targetFolder}`, "debug");
    }

    // Perform chosen archive action
    if (actionCode === "COPY_ADD_DATE") {
      const newFilePath = `${targetFolder}/${newTitle}.md`;
      await app.vault.create(newFilePath, originalContent);
      logStep("ARCH_5", `Copied file to ${newFilePath}`, "debug");
      if (openArchivedFile) {
        await app.workspace.openLinkText(newTitle, targetFolder, true);
      }
    } else if (actionCode === "MOVE_ADD_DATE") {
      const newFileName = `${targetFolder}/${newTitle}`;
      await tp.file.move(newFileName, currentFile);
      logStep("ARCH_5", `Moved + renamed file => ${newFileName}`, "debug");
    } else if (actionCode === "MOVE_NO_DATE") {
      const newFileName = `${targetFolder}/${originalTitle}`;
      await tp.file.move(newFileName, currentFile);
      logStep("ARCH_5", `Moved file => ${newFileName}`, "debug");
    }
    new Notice(`File archived to: ${targetFolder}`, 4000);

  } catch (err) {
    logStep("ARCH_ERR", `Archive flow error: ${err.message}`, "error");
  }
}

/******************************************************************************
 * CREATE NEW TEMPLATE FLOW
 ******************************************************************************/
async function createNewTemplate() {
  try {
    logStep("NEW-TPL_1", "Starting 'Create New Template' flow.", "debug");
    const allFolders = app.vault
      .getAllLoadedFiles()
      .filter(f => f instanceof tp.obsidian.TFolder && f.path.startsWith(templateDirectoryPath))
      .map(f => f.path);

    if (allFolders.length === 0) {
      logStep("NEW-TPL_2", "No subfolders found. Prompting user for manual folder path.", "debug");
      const manualFolder = await tp.system.prompt("No folders found. Enter folder path manually:");
      if (!manualFolder) {
        logStep("NEW-TPL_3", "User canceled manual folder input.", "debug");
        return;
      }
      allFolders.push(manualFolder);
    }

    const selectedFolder = await tp.system.suggester(
      allFolders,
      allFolders,
      false,
      "Select a folder for the new template:"
    );
    if (!selectedFolder) {
      logStep("NEW-TPL_4", "User canceled folder selection for new template.", "debug");
      return;
    }

    const templateName = await tp.system.prompt("Enter a name for the new template:");
    if (!templateName) {
      logStep("NEW-TPL_5", "User canceled template name input.", "debug");
      return;
    }
    const finalName        = `${templateName.trim()} - template`;
    const newTemplatePath  = `${selectedFolder}/${finalName}`;
    await tp.file.create_new("", finalName, false, selectedFolder);
    logStep("NEW-TPL_6", `Created new template: ${newTemplatePath}`, "debug");

    let newTemplateFile = app.vault.getAbstractFileByPath(newTemplatePath);
    if (!newTemplateFile) {
      newTemplateFile = tp.file.find_tfile(newTemplatePath);
    }
    if (newTemplateFile instanceof tp.obsidian.TFile) {
      await app.workspace.activeLeaf.openFile(newTemplateFile);
      const editor = app.workspace.activeLeaf.view?.sourceMode?.cmEditor;
      if (editor) editor.focus();
    } else {
      logStep("NEW-TPL_7", "Could not open newly created template file.", "warn");
    }
  } catch (error) {
    logStep("NEW-TPL_8", `Error creating new template: ${error.message}`, "error");
  }
}

/******************************************************************************
 * GLOBAL STATE
 ******************************************************************************/
let globalSecondaryTemplateContent = null;

/******************************************************************************
 * MAIN NOTE CREATION FLOW
 ******************************************************************************/
async function mainNoteCreationFlow() {
  try {
    logStep("MAIN-FLOW_1", "Begin Main Note Creation Flow...", "debug");
    const typePickResult = await pickTypeAndMaybeSubtype();
    if (!typePickResult.finalType) {
      logStep("MAIN-FLOW_2", "No final type chosen or advanced function triggered. Exiting.", "debug");
      return;
    }

    if (typePickResult.isCustom) {
      logStep("MAIN-FLOW_3", `User typed custom type: "${typePickResult.finalType}"`, "debug");
      const customConfig = { ...DEFAULT_TYPE_SETTINGS, type: typePickResult.finalType };
      const publish = await determinePublishStatus(customConfig);
      const action  = await selectAction();
      const fileName = (action === "new")
        ? await tp.system.prompt("Enter file name for new note:")
        : null;
      const newFile = await handleFileOperations(customConfig, action, fileName);
      await applyAllNoteChanges(customConfig, publish, globalSecondaryTemplateContent, newFile);
      return;
    }

    let finalTypeConfig = getTypeConfig(typePickResult.finalType);
    if (typePickResult.subtype) {
      finalTypeConfig = mergeParentChildConfigs(finalTypeConfig, typePickResult.subtype);
      logStep("MAIN-FLOW_5", `Merged subtype "${typePickResult.subtype}".`, "debug");
    }

    if (Array.isArray(finalTypeConfig.area) && finalTypeConfig.area.length > 0) {
      const chosenArea = await promptForAreaChoice(finalTypeConfig.area, typePickResult.finalType);
      finalTypeConfig.area = chosenArea || finalTypeConfig.area[0];
    }

    const publishStatus = await determinePublishStatus(finalTypeConfig);
    const action        = await selectAction();
    const fileName      = (action === "new")
      ? await tp.system.prompt("Enter file name for new note:")
      : null;

    const newOrMovedFile = await handleFileOperations(finalTypeConfig, action, fileName);
    await applyAllNoteChanges(finalTypeConfig, publishStatus, globalSecondaryTemplateContent, newOrMovedFile);

    logStep("MAIN-FLOW_6", "Main note creation flow complete.", "debug");

  } catch (err) {
    logStep("MAIN-FLOW_7", `Error in mainNoteCreationFlow: ${err.message}`, "error");
  }
}

/******************************************************************************
 * EXECUTION
 ******************************************************************************/
(async () => {
  try {
    logStep("EXEC_1", "Starting Global Note Template Execution...", "debug");
    await mainNoteCreationFlow();
  } catch (err) {
    logStep("EXEC_2", `Uncaught error: ${err.message}`, "error");
  }
})();


%>