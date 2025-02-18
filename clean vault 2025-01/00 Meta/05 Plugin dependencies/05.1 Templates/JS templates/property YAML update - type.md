<%*
const file3 = tp.file.find_tfile(tp.file.path(true));
const selectType = await tp.system.suggester((type) => type, ["article", "book", "movie", "note", "podcast episode", "recipe", "series"
]) ;

  await app.fileManager.processFrontMatter(file, (frontmatter) => {
    frontmatter["type"] = selectType;
  });
-%>