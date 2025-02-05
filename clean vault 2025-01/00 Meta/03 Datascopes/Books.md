---
up:
  - "[[Interests]]"
related:
  - "[[Media MOC]]"
  - "[[Meta bind datascope]]"
aliases:
  - Library - Books & Reading Lists
  - Library (datascope)
banner: https://i.imgur.com/zk2xuiP.png
banner_x: 0.5
banner_y: 0.66
cssclasses:
  - wide-dataview
  - dvl-c
date-created: 2024-10-09
publish: true
tags:
  - atlas/datascope
statusBind:
  - paused
  - not started
  - scheduled
---

> [!property]+ select **status**
>
> ```meta-bind
> INPUT[multiSelect(
> option(not started), 
> option(scheduled),
> option(active, reading), 
> option(paused), 
> option(waiting), 
> option(done), 
> option(cancelled)
> ):statusBind]
> ```
>  `BUTTON[resetStatusBind]`


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
    status
FROM "" 
    AND -"00 Meta"
WHERE 
    contains(type, "book") 
    AND (
        any(this.statusBind, (s) => contains(lower(list(status)), lower(s)))
    OR this.statusBind = null
    )
SORT date-created DESC
LIMIT 150
```


```meta-bind-button
label: "Reset status"
style: destructive
id: "resetStatusBind"
hidden: true
actions:
- type: updateMetadata
  bindTarget: statusBind
  evaluate: false
  value: null