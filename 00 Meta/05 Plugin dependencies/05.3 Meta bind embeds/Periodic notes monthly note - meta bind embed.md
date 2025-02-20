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

> [!link|no icon]+ `=link(dateformat(this.date-created - dur(1 month), "yyyy-MM"), dateformat(this.date-created - dur(1 month), "◀️ MMMM"))` `BUTTON[periodic-notes-date-switcher]` `= this.file.aliases` `=link(dateformat(this.date-created + dur(1 month), "yyyy-MM"), dateformat(this.date-created + dur(1 month), "MMMM ▶️"))`
> 
>> [!photo]- snapshot
>> 
>> - last month `=default(link(this.last-month, dateformat(this.date-created - dur(1 month), "MMMM")), "**last month**")` 
>>     - sleep-comments : `=default(this.last-month.sleep-comments, "**set last month's sleep-comments**")`
>>     - highlights : `=default(this.last-month.highlights, "**set last month's highlights**")`
>>     - lessons-learned : `=default(this.last-month.lessons-learned, "**set last month's lessons-learned**")`
>>     - next-plans : `=default(this.last-month.next-plans, "**set last month's next-plans**")`
>>     - theme : `=default(this.last-month.theme, "**set last month's theme**")`
>> - this month (`=this.file.name`)
>>     - theme : `=default(this.theme, "**set this months theme below**")`
>> - this year `=default(link(this.year), "**create this year's note**")`
>>     - theme : `=default(this.year.theme, "**set this year's theme**")` 
>>     - last year's (`=this.year.last-year.file.name`) next-plans : `=default(this.year.last-year.next-plans, "**set last year's next-plans**")`
>> 
>>> [!image-description] `=choice(this.banner, "![photo|300](" + this.banner + ")" + choice(this.banner-description, this.banner-description, ""), "**paste image URL into banner button below**")`
>>
>>%%  - `=link(this.quarter)` projects : `=this.quarter.projects` %%
>
>> [!reflection]- ### sleep
>> 
>> - insert sleep duration : `INPUT[number(placeholder(sleep duration)):sleep-duration]`
>> 
>> calculated sleep duration : `= "**" + round(sum(nonnull(this.weeks.sleep-duration)) / length(nonnull(this.weeks.sleep-duration)), 2) + " hours**"` 
>> 
>> `INPUT[textArea(placeholder('sleep comments : observations, patterns and reflections on sleep quality')):sleep-comments]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     "**" + weeks.sleep-duration + "** hours" AS "TotalSleepTime",
>>     weeks.sleep-comments AS "sleep comments"
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> SORT weeks.file.name DESC
>> LIMIT 10
>> ```
>>
>
>> [!reflection]- ### gratitude
>> 
>> `INPUT[textArea(placeholder('gratitude : what are 3 to 10 things that I am grateful for? what is something that I have never noticed in my immediate environment?')):gratitude]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     weeks.gratitude AS gratitude
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> SORT weeks.file.name DESC
>> LIMIT 10
>> ```
>
>> [!reflection]- ### theme
>> 
>> `INPUT[textArea(placeholder(theme)):theme]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     file.link AS date,
>>     theme
>> FROM #log/month
>> WHERE
>>     date-created >= this.date-created - dur(12 months)
>>     AND date-created < this.date-created
>> SORT weeks.file.name DESC
>> LIMIT 10
>> ```
>
>> [!reflection]- ### highlights
>> 
>> - `INPUT[textArea(placeholder('highlights : key moments? what excited me? how did I move things forward?')):highlights]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     weeks.highlights AS highlights
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> SORT weeks.file.name DESC
>> LIMIT 10
>> ```
>
>> [!reflection]- ### lessons-learned
>> 
>> `INPUT[textArea(placeholder('lessons learned : insights, mistakes, failures and a functional interpretation')):lessons-learned]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     weeks.lessons-learned AS lessons-learned
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> SORT weeks.file.name DESC
>> LIMIT 10
>> ```
>
>> [!reflection]- ### next-plans
>> 
>> `INPUT[textArea(placeholder('next-plans : plan the next period, set goals, and any tasks to do')):next-plans]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     weeks.next-plans AS next-plans
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> SORT weeks.file.name DESC
>> LIMIT 10
>> ```
>
>> [!photo]-
>> 
>> `BUTTON[add-banner-direct-image-URL]`
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date,
>>     choice(weeks.banner, "<br><sub><small>" + "[URL](" +  weeks.banner + ")" + "</small></sub>" +
>>     "![photo](" + weeks.banner + ")", "") 
>>      AS "" 
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> SORT weeks.file.name DESC
>> LIMIT 10
>> ```
>
>> [!search|c]- periodic notes support pages
>> 
>> ```meta-bind-embed
>> [[Periodic notes support pages - meta bind]]
>> ```
>
> [[Periodic notes monthly note - meta bind embed||📝 edit embed]] | [[Periodic notes - checklist - monthly|✔️edit review checklist]] | `BUTTON[monthly-review]`

```meta-bind-embed
[[Meta bind button index - Meta bind embed]]
```
