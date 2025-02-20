<%*
/*******************************************************************************
 * GLOBAL NOTE TEMPLATE (GNT)
 * Template for creating and managing different note types in Obsidian
 * Author: @dryice9290  (Discord). 

 ******************************************************************************/

/******************************************************************************
 * DEBUG ON/OFF SETTING
 * Set to "true" to enable debug & warn logs; "false" to only show errors.
 ******************************************************************************/
const DEBUG_ENABLED = true;

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
  new: {
    type: "new_type",
    path: "new-type-path"
  },
  note: {
    path: "30 Notes"
  },
  /**
  medicine: {
    type: "medical_note",
    path: "30 Notes/31 Medicine",
    area: "",
    template: "00 Meta/05 Plugin dependencies/05.1 Templates/Note structure templates/Templates relating to my workflow/Medicine - diagnosis.md",
    publishMode: "always"
  },
*/

  my: {
    type: "note",
    area: "my",
    path: "20 My/22 Reflections",
    publishMode: "never"
  },

  recipe: {
    type: "recipe",
    path: "30 Notes",
    template: "00 Meta/05 Plugin dependencies/05.1 Templates/Note structure templates/Recipe and food - template.md",
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
    path: "40 Projects/" + await tp.date.now("YYYY/[Q]Q"),
    template: "00 Meta/05 Plugin dependencies/05.1 Templates/Note structure templates/Project template - template.md",
    customMetadata: {
      quarter: "[[" + await tp.date.now("YYYY-[Q]Q") + "]]",
      priority: null,
      "outcome-goals": null,
      deadline: null,
    },
    publishMode: "prompt"
  },

  source: {
    type: "source",
    path: "10 Sources",
    publishMode: "always",
    subtypes: {
      book: {
          type: "book",
          template: "00 Meta/05 Plugin dependencies/05.1 Templates/Note structure templates/Mentions note - template.md",
          renameRule: { rule: "suffix", value: " - Book" },
      },
      article: { 
          type: "article",
          template: "",
      },
      website: { 
          type: "website",
          template: "",
      },
      LLM: { 
          type: "LLM",
          template: "",
      },
      podcast: { 
          type: "podcast",
          template: "",
          renameRule: { rule: "suffix", value: " - Podcast channel" } 
      },
      podcast_episode: { 
          type: "podcast_episode",
          template: "",
          renameRule: { rule: "suffix", value: " - Podcast episode" } 
      },
      YouTube: { 
          type: "YouTube",
          template: "",
          renameRule: { rule: "suffix", value: " - YouTube" } 
      },
      film: { 
          type: "film",
          template: "",
          renameRule: { rule: "suffix", value: " - Film" } 
      },
      series: { 
          type: "series",
          template: "",
          },
      author:  {
          type: "author",
          template: "",
      }
    },
    customMetadata: {
      author: null,
      publisher: null,
      'date-published': null, 
      url: null,
      source: null,
      status: null,
      rating: null
    }
  },

  person: {
    type: "person",
    path: "20 My/24 People",
    template: "00 Meta/05 Plugin dependencies/05.1 Templates/Note structure templates/Mentions note - template.md",
    customMetadata: {
      up: "[[Contacts - datascope]]",
      birthdate: null,
      "contact-frequency": null
    },
    publishMode: "never"
  },
  };

/** Standard metadata merged into final frontmatter. */
const STANDARD_METADATA = {
  up: [],   // 👈 Ensure it starts as an empty array instead of null
  related: [],
  aliases: [],
  "date-created": await tp.date.now("YYYY-MM-DD"),
  banner: null,
  cover: null,
  "banner-y": null,
  tags: [],
  publish: false,
  type: [],
  area: [],
  summary: null
};

/** Keys that are always formatted as YAML arrays. */
const listKeys = [
  "up",
  "related",
  "quarter",
  "aliases",
  "tags",
  "type",
  "area",
  "cuisine"
];

/** FIELD-BY-FIELD METADATA POLICY */ 
 // * For each key, define how the new template value should be merged.
const METADATA_POLICY = {
  // Example field behaviors:
  
  "date-created":   "ignoreIfSet",
  "author":         "appendArray",
  "up":         "appendArray",
  "publish":        "alwaysOverwrite",
  "type":           "alwaysOverwrite",
  "area":         "alwaysOverwrite",
  
  // etc. Add more entries for any keys you want special handling.
};


/** Secondary templates directory path. */
const templateDirectoryPath = "00 Meta/05 Plugin dependencies/05.1 Templates/";

/** Where to insert the type template (if used) - e.g. after frontmatter = true, append to note = false. */
const TEMPLATE_INSERT_LOCATION_AFTER_FRONTMATTER = true;

/** prompt for template insertion prompt rule - e.g. prompt for template = false, auto insert template = true */
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
 * Wrap string values in double quotes so that array items appear as "- \"...\""
 ******************************************************************************/
function buildYamlString(metadata) {

  // Helper to wrap any string value in double quotes
  function quoted(val) {
    // For simplicity, use JSON.stringify, which returns a valid JS string literal.
    // e.g. [[Home]] => "[[Home]]"
    if (typeof val === "string") {
      return JSON.stringify(val);
    }
    return val;
  }

  const lines = ["---"];
  for (const [key, value] of Object.entries(metadata)) {
    if (value === null || value === undefined) {
      lines.push(`${key}:`);
    } else if (Array.isArray(value)) {
      lines.push(`${key}:`);
      for (const item of value) {
        lines.push(`  - ${quoted(item)}`);
      }
      } else {
      lines.push(`${key}: ${value}`);
      }
  }
  lines.push("---");
  return lines.join("\n");
}

/******************************************************************************
 * HELPER: RENAME IF UNTITLED AND EMPTY
 ******************************************************************************/
async function renameIfUntitledAndEmpty() {
  try {
    const currentFile = tp.file.find_tfile(tp.file.path(true));
    if (!currentFile) return null;

    const content = await app.vault.read(currentFile);
    const isEmpty = content.trim().length === 0;
    const titleOnly = tp.file.title;
    const untitledRegex = /^Untitled(?:\s+\d+)?$/;
    if (!untitledRegex.test(titleOnly) || !isEmpty) return null;

    // Prompt user for the new name
    const newTitle = await tp.system.prompt(
      `Edit file title '${titleOnly}':`
    );
    if (!newTitle) return null;

    // Check collisions (simple version)
    const folderPath = await tp.file.folder(true);
    const newPath = `${folderPath}/${newTitle}.md`;
    if (await tp.file.exists(newPath)) {
      new Notice(`A file named '${newTitle}.md' already exists. Rename aborted.`, 5000);
      return null;
    }

    // Actually rename
    await tp.file.move(newPath, currentFile);
    logStep("RENAME_6", `Renamed file from '${titleOnly}' to '${newTitle}'.`, "debug");

    // [FIX for rename timing]
    // Store the new *base* name in a global variable
    // e.g. "newTitle" without ".md"
    globalUserRenamedBaseName = newTitle;

    return newTitle;

  } catch (err) {
    logStep("RENAME_ERR", `Error in renameIfUntitledAndEmpty: ${err.message}`, "error");
    return null;
  }
}

/******************************************************************************
 * ACTION SELECTION (APPLY OR CREATE)
 ******************************************************************************/
async function selectAction() {
  try {
    logStep("SEL-ACT_1", "Entered selectAction()...", "debug");

    // Try auto-rename if "Untitled <N>" & empty
    const renamedTitle = await renameIfUntitledAndEmpty();
    if (renamedTitle) {
      logStep("SEL-ACT_2", `Auto-renamed to '${renamedTitle}'. Forcing "current" action.`, "debug");
      return "current";  // skip the standard prompt
    }

    // Otherwise, fallback to standard behavior
    logStep("SEL-ACT_3", "Prompting user: Apply or Create new note?", "debug");
    const actionOptions = ["Apply to current note", "Create new note"];
    const choice = await tp.system.suggester(actionOptions, actionOptions, false, 
      "Select the desired action:"
    );
    if (!choice) throw new Error("Action selection canceled by user");
    logStep("SEL-ACT_4", `Action chosen: ${choice}`, "debug");
    return choice === "Create new note" ? "new" : "current";

  } catch (err) {
    logStep("SEL-ACT_ERR", `Error selecting action: ${err.message}`, "error");
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
      // [FIX for rename timing]:
      // If an auto-rename occurred, we have stored the new base name in globalUserRenamedBaseName.
      // We need to use that instead of relying on tp.file.title (which may be stale).
      const currentFile = tp.file.find_tfile(tp.file.path(true));
      if (!currentFile) {
        throw new Error("No current file found!");
      }
      
      // Determine the base name: use the auto-renamed name if available,
      // otherwise fallback to the current TFile's basename.
      let baseName;
      if (globalUserRenamedBaseName) {
        baseName = globalUserRenamedBaseName;
        logStep("FILEOPS_2", `Using auto-renamed base name: "${baseName}"`, "debug");
      } else {
        baseName = currentFile.basename.replace(/\.md$/, "");
        logStep("FILEOPS_2", `Using fallback base name: "${baseName}"`, "debug");
      }
      
      // Apply naming convention (e.g. prefix/suffix rules)
      const renamed = applyNamingConvention(typeConfig, baseName);
      
      // Build the final target file path
      targetFilePath = `${folderPath}/${renamed}`;
      if (await tp.file.exists(targetFilePath)) {
        throw new Error("Destination file already exists!");
      }
      
      // Move the file to the final target path
      await tp.file.move(targetFilePath);
      logStep("FILEOPS_5", `Moved current file to: ${targetFilePath}`, "debug");
      
      // Re-fetch the TFile for the new target file
      finalTFile = tp.file.find_tfile(targetFilePath);
      
      // Optionally reset the global variable for future runs
      globalUserRenamedBaseName = null;
    }
    
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
 * HELPER: MERGE FRONTMATTER WITH PER-FIELD POLICY (with proper array appending)
 ******************************************************************************/
function mergeWithPolicy(existing, incoming, policyMap) {
  const result = { ...existing };

  for (const [key, newValue] of Object.entries(incoming)) {
    const policy = policyMap[key] || "takeIfEmpty";
    const oldValue = result[key];

    switch (policy) {
      case "appendArray": {
        // Skip if incoming is null/undefined
        if (newValue == null) {
          break;
        }

        // Convert existing value to an array
        let oldArr = [];
        if (Array.isArray(oldValue)) {
          oldArr = [...oldValue];
        } else if (typeof oldValue === "string" && oldValue.trim() !== "") {
          oldArr = [oldValue];
        }

        // Convert new value to an array
        let newArr = [];
        if (Array.isArray(newValue)) {
          newArr = [...newValue];
        } else if (typeof newValue === "string" && newValue.trim() !== "") {
          newArr = [newValue];
        }

        // Combine, then remove duplicates
        // A Set will remove exact string duplicates
        const combinedUnique = [...new Set([...oldArr, ...newArr])];

        // Assign final result
        result[key] = combinedUnique;
        break;
      }

      case "ignoreIfSet": {
        // Only set if existing is null, undefined, empty string, or empty array
        const isEmptyString = typeof oldValue === "string" && oldValue.trim() === "";
        const isEmptyArray  = Array.isArray(oldValue) && oldValue.length === 0;

        if (oldValue == null || isEmptyString || isEmptyArray) {
          result[key] = newValue;
        }
        break;
      }

      case "alwaysOverwrite": {
        result[key] = newValue;
        break;
      }

      case "takeIfEmpty":
      default: {
        // If existing is null/undefined/empty, set newValue; otherwise keep oldValue
        if (
          !oldValue ||
          (typeof oldValue === "string" && oldValue.trim() === "") ||
          (Array.isArray(oldValue) && oldValue.length === 0)
        ) {
          result[key] = newValue;
        }
      }
    }
  }

  return result;
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
    logStep("APPLY_1", "Starting single-pass write for note...", "debug");

    // 1) Identify the note file we will modify
    let file = targetFile;
    if (!file) {
      file = tp.file.find_tfile(tp.file.path(true));
      if (!file) {
        throw new Error("Could not find a file to update frontmatter.");
      }
    }
    logStep("APPLY_2", `Using file: ${file.path}`, "debug");

    // 2) Read the file's current content
    const existingContent = await app.vault.read(file);
    logStep("APPLY_3", "Read existing file content.", "debug");

    // 3) Try extracting existing frontmatter
    const yamlRegex = /^---\s*\n([\s\S]*?)\n---\s*\n?/;
    let existingFrontmatter = {};
    let existingBody = existingContent;
    const match = yamlRegex.exec(existingContent);

    if (match) {
      // Use Obsidian's processFrontMatter to read the existing frontmatter object
      await app.fileManager.processFrontMatter(file, fm => {
        existingFrontmatter = { ...fm };
      });
      existingBody = existingBody.replace(yamlRegex, "").trim();
      logStep("APPLY_4", "Extracted existing frontmatter from note.", "debug");
    } else {
      logStep("APPLY_4", "No existing frontmatter found. Using empty object.", "debug");
    }

    // 4) Combine (STANDARD_METADATA + typeConfig.customMetadata) => incomingData
    let incomingData = { ...STANDARD_METADATA };
    if (typeConfig.customMetadata) {
      incomingData = { ...incomingData, ...typeConfig.customMetadata };
      logStep("APPLY_5", "Merged custom metadata from typeConfig.", "debug");
    }

    // 5) Merge with per-field policy
    //    (So we only overwrite fields if policy says so)
    let metadataToAdd = mergeWithPolicy(existingFrontmatter, incomingData, METADATA_POLICY);
    logStep("APPLY_6", "Merged existing frontmatter with incoming data via policy.", "debug");

    // 6) Force or override specific fields (publish, type, area)
    metadataToAdd.publish = publishStatus;
    logStep("APPLY_7", `Set 'publish' = ${publishStatus}`, "debug");

    if (typeConfig.type) {
      metadataToAdd.type = typeConfig.type;
      logStep("APPLY_7", `Set 'type' = ${typeConfig.type}`, "debug");
    }
    if (typeConfig.area) {
      metadataToAdd.area = typeConfig.area;
      logStep("APPLY_7", `Set 'area' = ${typeConfig.area}`, "debug");
    }

    // 7) Convert certain keys to arrays if they are not already arrays
    listKeys.forEach(key => {
      if (metadataToAdd.hasOwnProperty(key) && metadataToAdd[key] !== null) {
        if (!Array.isArray(metadataToAdd[key])) {
          metadataToAdd[key] = [metadataToAdd[key]];
          logStep("APPLY_8", `Converted frontmatter key "${key}" to array.`, "debug");
        }
      }
    });

    // 8) Possibly read type template, if not using auto-insert
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
        logStep("APPLY_9", `User opted to insert type template from "${typeConfig.template}".`, "debug");
      } else {
        logStep("APPLY_9", "User chose not to insert type template.", "debug");
      }
    } else if (typeConfig.template && AUTO_INSERT_TYPE_TEMPLATE) {
      typeTemplateContent = await maybeReadFileContent(typeConfig.template);
      logStep("APPLY_9", `Auto-inserted type template from "${typeConfig.template}".`, "debug");
    }

    // 9) Build final body content
    let finalBody = existingBody.trim();
    if (secondaryTemplateContent) {
      finalBody += `\n\n${secondaryTemplateContent.trim()}`;
      logStep("APPLY_10", "Appended secondaryTemplateContent to body.", "debug");
    }
    if (typeTemplateContent) {
      if (TEMPLATE_INSERT_LOCATION_AFTER_FRONTMATTER) {
        finalBody = `${typeTemplateContent.trim()}\n\n${finalBody}`;
        logStep("APPLY_10", "Inserted type template content at top (after frontmatter).", "debug");
      } else {
        finalBody += `\n\n${typeTemplateContent.trim()}`;
        logStep("APPLY_10", "Appended type template content at the end of body.", "debug");
      }
    }

    // 10) Reassemble final frontmatter + body
    const finalYaml = buildYamlString(metadataToAdd);
    const finalContent = `${finalYaml}\n\n${finalBody}`.trim();
    logStep("APPLY_11", "Reassembled final YAML + body.", "debug");

    // 11) Write it all back to the file in one pass
    await app.vault.modify(file, finalContent);
    logStep("APPLY_12", "Wrote updated frontmatter & body to file.", "debug");

  } catch (err) {
    logStep("APPLY_ERR", `Failed to apply note changes: ${err.message}`, "error");
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
let globalUserRenamedBaseName = null;

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