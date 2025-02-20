<%*
// Remove the entire link when in the form of a reference with a digit as a display, like [1](some-url.com)

const content = tp.file.content
const striplinks = content.replace(/(?<!\!)\[(.+)\]\(([^ ]+?)( "(.+)")?\)/g, "$1")
const file = tp.file.find_tfile(tp.file.title)
await app.vault.modify(file, striplinks)
%>