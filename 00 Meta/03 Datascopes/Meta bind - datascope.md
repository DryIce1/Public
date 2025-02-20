---
up:
  - "[[Meta bind embeds (MOC, datascope)]]"
related:
  - "[[Meta bind datascope - embed]]"
aliases: 
date-created: 2024-10-21
banner: https://www.wallart.com/media/catalog/product/cache/871f459736130e239a3f5e6472128962/b/e/behang-van-een-bos-met-lichte-nevel_8.jpg
banner-y: 120
cssclasses:
  - wide-dataview
  - dvl-c
publish: true
searchBind: 
authorBind: 
folderBind: 
typeBind: 
statusBind: 
urgentBind: false
importantBind: false
ratingBind: 
date-createdBind: 
dateRangeYearsBind: 
dateRangeDaysBind: 
limitBind: 
sortBind: 
groupBind: 
priorityBind: 
coverBind: false
type: datascope
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
# Filters
```meta-bind-embed
[[Meta bind datascope - embed]]
```

# Query

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
    any(
        this.typeBind,
        this.authorBind,
        this.statusBind,
        this.ratingBind,
        this.searchBind,
        this.date-createdBind,
        this.folderBind,
        this.sortBind,
        (this.coverBind != false),
        (this.urgentBind != false),
        (this.importantBind != false)
    )

    AND 
        (
        all(this.searchBind, (sB) =>
            (
            contains(lower(file.name), lower(sB))
            OR contains(lower(file.etags), lower(sB))
            OR any(file.lists, (s) => contains(lower(s.text), lower(sB)))
            )
        ) 
        OR this.searchBind = null
        )
    
    AND 
        (
          any(this.authorBind, (s) => contains(lower(list(author)), lower(s)))
          OR this.authorBind = null
        )

AND (
  (
    any(this.folderBind, (f) => contains(lower(file.folder), lower(f)))
  )
  OR this.folderBind = null
)

    AND 
        (
          any(this.typeBind, (s) => contains(lower(list(type)), lower(s)))
          OR this.typeBind = null
        )

    AND 
    (
        any(this.statusBind, (s) => contains(lower(list(status)), lower(s)))
    OR this.statusBind = null
    )

   AND 
    (
    (this.urgentBind = true AND urgent = true)
    OR this.urgentBind != true
    )

AND 
    (
    (this.importantBind = true AND important = true)
    OR this.importantBind != true
    )

AND 
    (
    (this.coverBind = true AND default(cover, banner) != null)
    OR this.coverBind != true
    )

    AND 
    (
        (
            rating >= (default(number(this.ratingBind), 0) - 1) 
            AND rating <= (default(number(this.ratingBind), 7) + 1)
        )
    OR this.ratingBind = null
    )

    AND 
(
    (
        date-created >= (
            default(date(this.date-createdBind), date(today)) 
            - dur(default(number(this.dateRangeYearsBind), 0) + " years") 
            - dur(default(number(this.dateRangeDaysBind), 90) + " days")
        )
        AND 
        date-created <= (
            default(date(this.date-createdBind), date(today))
            + dur(default(number(this.dateRangeYearsBind), 0) + " years") 
            + dur(default(number(this.dateRangeDaysBind), 90) + " days")
        )
    )
    OR this.date-createdBind = null
)
SORT date-finished DESC
SORT date-created DESC
LIMIT default(this.limitBind, 150)
```
