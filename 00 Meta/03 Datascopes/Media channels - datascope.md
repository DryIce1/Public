---
up:
  - "[[Media MOC]]"
related: 
aliases: 
banner: https://i.imgur.com/BfN9jFb.jpg
banner_y: 1
cssclasses:
  - wide-dataview
  - dvl-c
date-created: 2025-01-17
publish: true
banner-y: 
cover: 
type:
  - datascope
area: 
summary: 
---

```dataview
TABLE 
    WITHOUT ID
    choice(
        none(default(cover, banner), false), 
        "<img align='left' width='120' src='https://assets.streamlinehq.com/image/private/w_300,h_300,ar_1/f_auto/v1/icons/all-icons/book-open-ztzdkpcbmp1or0of1vvcz.png/book-open-lz80u7pi1jc99bzltizkof.png?_a=DAJFJtWIZAAC'>", 
        "<img align='left' width='120' src='" + default(cover, banner) + "'>") 
AS "Cover",
    choice(title, link(file.link, title), file.link) 
    + 
    ""
    +
    choice(nonnull(rating), default(" ⭐" + "*" + rating + "/7*", ""), null)
    + 
    choice(none(author, false), "", "<br> by *" + author + "*") 
AS "file names & [[Rating scale]]",
        "" + 
        choice(none(type, false), "", "<sub>type: " + type + "</sub>") + 
        choice(none(status, false), "", "<br><sub>status: " + status + "</sub> ") + 
    "" + 
    choice(
        !contains(status, "done"), 
        choice(
            (urgent = null AND important = null), 
            "",  
            "<br><sub>" + 
                choice(urgent, "**urgent**", "non-urgent") + 
                " & " + 
                choice(important, "**important**", "non-important") + 
            "</sub>"
        ),
        ""
    ) 
    +
    "<br><sub>(*" + 
        choice(
            none(date-finished, false), 
            "created: " + dateformat(default(date-created, file.cday), "yyyy, MMM"), 
            "finished: " + dateformat(date-finished, "yyyy, MMM")
        ) + 
    "*)" + choice(nonnull(this.folderbind[0]), "<br>· " + file.folder, null) + "</sub>" 


    AS description
FROM "" 
    AND -"00 Meta"
WHERE 
    contains(type, "podcast_channel") OR
    contains(type, "film") OR
    contains(type, "series") OR
    contains(type, "YouTube_channel") 
    AND (
        any(this.statusBind, (s) => contains(lower(list(status)), lower(s)))
    OR this.statusBind = null
    )
SORT date-created DESC
LIMIT 50
```
