<%*
const clipboardUrl = await tp.system.clipboard()
    try {
        // Prompt the user for the banner image description
        const bannerDescription = await tp.system.prompt("banner description or caption", "description" )

        // processFrontMatter adds 'banner' and 'banner-description' properties to the current note's YAML frontmatter
        await app.fileManager.processFrontMatter(app.workspace.getActiveFile(), (frontmatter) => {
            frontmatter['banner'] = clipboardUrl.trim();
            frontmatter['banner-description'] = bannerDescription; 
        });
        
    // Truncate the clipboard URL for the notice (first 40 characters, add ellipsis if longer)
    const trimmedUrl = clipboardUrl.trim();
    const truncatedUrl = trimmedUrl.length > 40 ? trimmedUrl.substring(0, 50) + "..." : trimmedUrl;
    
    // Show the success notice with a 10-second duration.
    new Notice("Banner updated successfully! \n\n" + bannerDescription + "\n\n" + truncatedUrl, 10000);
    
    } catch (e) {
        // Handle any errors that occur while updating the frontmatter
        tR += "An error occurred while trying to update the YAML frontmatter. Please try again.";
    }
%>