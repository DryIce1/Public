<%*
const file = tp.file.find_tfile(tp.file.path(true));
await app.fileManager.processFrontMatter(file, (frontmatter) => {
  frontmatter["banner"] = ""; // create a new property or update an existing one by name
  frontmatter[""] = "";
});
-%>