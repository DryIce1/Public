---
up:
date-created: 2024-11-11
cssclasses:
  - log
tags:
  - meta-bind-embed
---

```meta-bind-embed
[[Shortcuts widget - meta bind]]
```

> [!link|no icon]- `=link(dateformat(this.date-created - dur(1 year), "yyyy"), dateformat(this.date-created - dur(1 year), "◀️ yyyy"))` `BUTTON[periodic-notes-date-switcher]` `= this.file.name + choice(this.aliases, " : **" + this.aliases + "**",  "")` `=link(dateformat(this.date-created + dur(1 year), "yyyy"), dateformat(this.date-created + dur(1 year), "yyyy ▶️"))`
> 
>> [!photo]+ snapshot
>> 
>> - last year `=default(link(this.last-year, dateformat(this.date-created - dur(1 year), "yyyy")), "**last year**")` 
>>     - theme : `=default(this.last-year.theme, "**set last year's theme**")`
>>     - highlights : `=default(this.last-year.highlights, "**set last year's theme**")`
>>     - next-plans : `=default(this.last-year.next-plans, "**set last year's theme**")`
>> - this year (`=this.file.name`)
>>     - theme : `=default(this.theme, "**enter theme below**")`
>>     - next-plans : `=default(this.next-plans, "**enter next-plans below**")`
>> - this 5-year `=default(link(this["5-year"]), "**create this 5-year's note**")`
>>     - theme : `=default(this["5-year"].theme, "**set this 5-year's theme**")` 
>>     - next-plans : `=default(this["5-year"].next-plans, "**set this 5-year's theme**")`
>> 
>>> [!image-description] `=choice(this.banner, "![photo|300](" + this.banner + ")" + choice(this.banner-description, this.banner-description, ""), "**paste image URL into banner button below**")`
>
>> [!reflection]- ### sleep
>>
>> - insert sleep duration : `INPUT[number(placeholder(sleep duration)):sleep-duration]`
>> 
>> calculated sleep duration : `= "**" + round(sum(nonnull(this.months.sleep-duration)) / length(nonnull(this.months.sleep-duration)), 2) + " hours**"`
>> 
>> `INPUT[textArea(placeholder('sleep comments : observations, patterns and reflections on sleep quality')):sleep-comments]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     "**" + months.sleep-duration + "** hours" AS "TotalSleepTime",
>>     months.sleep-comments AS "sleep comments"
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> SORT months.file.name DESC
>> LIMIT 20
>> ```
>
>> [!reflection]- ### gratitude
>> 
>> `INPUT[textArea(placeholder('gratitude : what are 3 to 10 things that I am grateful for? what is something that I have never noticed in my immediate environment?')):gratitude]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.gratitude AS gratitude
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> SORT months.file.name DESC
>> LIMIT 20
>> ```
>
>> [!reflection]- ### theme
>> 
>> `INPUT[textArea(placeholder(theme)):theme]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.theme AS theme
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> SORT months.file.name DESC
>> LIMIT 20
>> ```
>
>> [!reflection]- ### highlights
>> 
>> - highlights : `INPUT[textArea(placeholder('highlights : key moments? what excited me? how did I move things forward?')):Highlights]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.highlights AS highlights
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> SORT months.file.name DESC
>> LIMIT 20
>> ```
>
>> [!reflection]- ### lessons-learned
>> 
>> `INPUT[textArea(placeholder('lessons learned : insights, mistakes, failures and a functional interpretation')):lessons-learned]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.lessons-learned AS lessons-learned
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> SORT months.file.name DESC
>> LIMIT 20
>> ```
>
>> [!reflection]- ### next-plans
>> 
>> `INPUT[textArea(placeholder('next-plans : plan the next period, set goals, and any tasks to do')):next-plans]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.next-plans AS next-plans
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> SORT months.file.name DESC
>> LIMIT 20
>> ```
>
>> [!photo]-
>> 
>> `BUTTON[add-banner-direct-image-URL]`
>> 
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     months.file.link AS date,
>>     choice(months.banner, "<br><sub><small>" + "[URL](" +  months.banner + ")" + "</small></sub>" +
>>     "![photo](" + months.banner + ")", "") 
>>      AS "" 
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> SORT months.file.name DESC
>> LIMIT 20
>> ```
>
>> [!search|c]- periodic notes support pages
>> 
>> ```meta-bind-embed
>> [[Periodic notes support pages - meta bind]]
>> ```
>
> [[Periodic notes yearly note - meta bind embed||📝 edit embed]] | [[Periodic notes - checklist - yearly - last year|✔️edit last-year's review checklist]] | [[Periodic notes - checklist - yearly - next year|✔️ edit next year's review checklist]] | `BUTTON[yearly-review-last-year]` | `BUTTON[yearly-review-next-year]`

```meta-bind-embed
[[Meta bind button index - Meta bind embed]]
```
