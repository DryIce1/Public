<%*
tp.hooks.on_all_templates_executed(async () => {
  const file = tp.file.find_tfile(tp.file.path(true));
  await tp.app.fileManager.processFrontMatter(file, (frontmatter) => {
    // To set the sleep or awake default value update the default time here
    frontmatter["sleep"] = tp.date.now("YYYY-MM-DD" + "T23:00:00", -1, tp.file.title, "YYYY-MM-DD");
    frontmatter["awake"] = tp.date.now("YYYY-MM-DD" + "T07:00:00", 0, tp.file.title, "YYYY-MM-DD");
  });
});

%>

