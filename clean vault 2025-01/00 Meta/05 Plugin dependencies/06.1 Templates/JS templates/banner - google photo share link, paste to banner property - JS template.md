<%*
// Step 1: Prompt for the Google Photos share link
const link = await tp.system.prompt('Enter the Google Photos share link')

// Verify if the link is a valid Google Photos share link
if (link && link.startsWith('https://photos.app.goo.gl/')) {
    try {
        // Fetch the page HTML using the provided Google Photos link
        const raw = await requestUrl({ url: link })
        const html = raw.text

        // Extract the direct permanent image URL from the HTML
        // The image URL matches the pattern starting with "https://lh3.googleusercontent.com"
        const imageUrlPattern = new RegExp('<img[^>]*?src="(https://lh3.googleusercontent.com[^"]+)=w\\d+-h\\d+-no"[^>]*?>', 's')
        const imageMatch = html.match(imageUrlPattern)

        if (imageMatch && imageMatch[1]) {
            // Get the permanent image URL and set a larger resolution
            const width = 1920;  // Set to a larger width (e.g., Full HD width)
            const height = 1080; // Set to a larger height (e.g., Full HD height)
            const permanentImageUrl = `${imageMatch[1]}=w${width}-h${height}-no`;

            // Use processFrontMatter to add the 'banner' property to the current note's YAML frontmatter
            await app.fileManager.processFrontMatter(app.workspace.getActiveFile(), (frontmatter) => {
                frontmatter['banner'] = permanentImageUrl; // Set the 'banner' property with the larger image URL
            });
        } else {
            // Handle the case where the image URL could not be found
            tR += "Could not find a valid image URL from the provided Google Photos link."
        }
    } catch (e) {
        tR += "An error occurred while trying to fetch the image URL. Please check the link and try again."
    }
} else {
    tR += "Invalid Google Photos share link. Please provide a valid link."
}
%>
