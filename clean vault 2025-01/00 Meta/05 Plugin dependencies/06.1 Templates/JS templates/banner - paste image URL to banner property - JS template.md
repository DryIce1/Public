<%*
// Step 1: Get the URL from the clipboard
const clipboardUrl = await tp.system.clipboard()

// Step 2: Verify if the URL is valid and starts with "https://lh3.googleusercontent.com"
if (clipboardUrl && clipboardUrl.startsWith('https://lh3.googleusercontent.com')) {
    try {
        // Step 3: Prompt the user for the banner image description
        const bannerDescription = await tp.system.prompt("Input banner image description")

        // Step 4: Use processFrontMatter to add the 'banner' and 'banner-description' properties to the current note's YAML frontmatter
        await app.fileManager.processFrontMatter(app.workspace.getActiveFile(), (frontmatter) => {
            frontmatter['banner'] = clipboardUrl; // Set the 'banner' property with the clipboard URL
            frontmatter['banner-description'] = bannerDescription; // Set the 'banner-description' property with the user's input
        });
    } catch (e) {
        // Handle any errors that occur while updating the frontmatter
        tR += "An error occurred while trying to update the YAML frontmatter. Please try again.";
    }
} else {
    // If the clipboard doesn't contain a valid Google Photos URL, return an error message
    tR += "Invalid URL. Please ensure the URL starts with 'https://lh3.googleusercontent.com'.";
}
%>