<%*
// Step 1: Get the URL from the clipboard
const clipboardUrl = await tp.system.clipboard()
try {
    // Step 2: Prompt the user for the banner image description
    // const bannerDescription = await tp.system.prompt("banner description or caption" )

    // Step 3: Use processFrontMatter to add the 'banner' and 'banner-description' properties to the current note's YAML frontmatter
    await app.fileManager.processFrontMatter(app.workspace.getActiveFile(), (frontmatter) => {
        frontmatter['banner'] = clipboardUrl; // Set the 'banner' property with the clipboard URL
        // frontmatter['banner-description'] = bannerDescription; // Set the 'banner-description' property with the user's input
    });
} catch (e) {
    // Handle any errors that occur while updating the frontmatter
    tR += "An error occurred while trying to update the YAML frontmatter. Please try again.";
}
%>