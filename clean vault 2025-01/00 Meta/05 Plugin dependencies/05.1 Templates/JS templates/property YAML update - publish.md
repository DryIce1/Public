<%*
const file2 = tp.file.find_tfile(tp.file.path(true));
const publishDecision = await tp.system.suggester(["Publish", "Don't publish"], [true, false]);
  await app.fileManager.processFrontMatter(file2, (frontmatter) => {
    frontmatter["publish"] = publishDecision;
  });
-%>