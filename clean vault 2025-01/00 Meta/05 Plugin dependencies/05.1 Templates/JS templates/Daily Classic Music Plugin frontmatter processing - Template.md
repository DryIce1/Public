<%*
/****************************
Daily Classic Music Plugin frontmatter processing- Template
 ****************************/

const content = tp.file.content;

// Regular expression to match the Daily Classical Music callout
const calloutRegex = /\n> \[!tip\] Daily Classical Music\n> \[(.+?)\]\((https:\/\/www\.youtube\.com\/watch\?v=.+?)\)\n/;

// Regular expression to match the meta-bind button code block
const buttonRegex = /\n```meta-bind-button\n[\s\S]*?```\n/;

// Extracting title and link from the callout
const match = content.match(calloutRegex);
if (match) {
  const title = match[1];
  const url = match[2];
  
  // Get current frontmatter and modify it
  const file = tp.file.find_tfile(tp.file.path(true));
  let frontmatter = tp.frontmatter;

  frontmatter['daily_classical_music_title'] = title;
  frontmatter['daily_classical_music_url'] = url;

  // Removing the callout and button block from the file content
  let updatedContent = content
    .replace(calloutRegex, "")  // Remove the callout
    .replace(buttonRegex, "");  // Remove the meta-bind button block

  // Check if frontmatter is already present in the content
  const frontmatterRegex = /^---\n[\s\S]*?\n---\n/;
  const frontmatterString = "---\n" + Object.entries(frontmatter)
    .map(([key, value]) => `${key}: ${value}`)
    .join("\n") + "\n---\n\n";

  // Replace the existing frontmatter or add it if not present
  if (frontmatterRegex.test(updatedContent)) {
    updatedContent = updatedContent.replace(frontmatterRegex, frontmatterString);
  } else {
    updatedContent = frontmatterString + updatedContent;
  }

  // Write the updated content back to the file
  await app.vault.modify(file, updatedContent);

  console.log('Successfully extracted, updated the frontmatter, and removed the desired content.');
} else {
  console.error('No Daily Classical Music callout found in the note.');
}
%>