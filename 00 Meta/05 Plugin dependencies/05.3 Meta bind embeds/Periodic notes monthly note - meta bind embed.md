---
up:
  - "[[Meta bind embeds (MOC, datascope)]]"
date-created: 2024-11-11
cssclasses:
  - log
tags:
  - meta-bind-embed
---

> [!link|no icon]- `=link(dateformat(this.date-created - dur(1 month), "yyyy-MM"), dateformat(this.date-created - dur(1 month), "◀️ MMMM"))` `BUTTON[periodic-notes-date-switcher]` `= this.file.aliases` `=link([[Home]], "🏠")` `=link(dateformat(this.date-created + dur(1 month), "yyyy-MM"), dateformat(this.date-created + dur(1 month), "MMMM ▶️"))`
> 
>> [!photo]+ snapshot
>> - this month
>>     - theme : `=default(this.theme, "**enter theme below**")`
>> - `=link(this.last-month, dateformat(this.date-created - dur(1 month), "MMMM"))` 
>>     - sleep-comments : `=this.last-month.sleep-comments`
>>     - highlights : `=this.last-month.highlights`
>>     - lessons learned : `=this.last-month.lessons-learned`
>>     - next-plans : `=this.last-month.next-plans`
>>     - theme : `=this.last-month.theme`
>> - `=this.year`
>>     - theme : `=this.year.theme` 
>>     - `=this.year.last-year.file.name` : `=this.year.last-year.next-plans`
>> 
>>> [!image-description] `=choice(this.banner, "![photo](" + this.banner + ")" + choice(this.banner-description, this.banner-description, ""), "**choose monthly banner**")`
>>
>>%%  - `=link(this.quarter)` projects : `=this.quarter.projects` %%
>
>> [!reflection]- ### sleep
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     "**" + weeks.sleep-duration + "** hours" AS "TotalSleepTime",
>>     weeks.sleep-comments AS "sleep comments"
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> sort file.name ASC
>> LIMIT 10
>> ```
>>
>> `INPUT[textArea(placeholder('sleep comments : observations, patterns and reflections on sleep quality')):sleep-comments]`
>> - average sleep duration : `=round(sum(nonnull(this.weeks.sleep-duration)) / length(nonnull(this.weeks.sleep-duration)), 2)` hours  `INPUT[number(placeholder(sleep duration)):sleep-duration]`
>
>> [!reflection]- ### gratitude
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     weeks.gratitude AS gratitude
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> sort file.name ASC
>> LIMIT 10
>> ```
>> `INPUT[textArea(placeholder('gratitude : what are 3 to 10 things that I am grateful for? what is something that I have never noticed in my immediate environment?')):gratitude]`
>
>> [!reflection]- ### theme
>> ```dataview
>> TABLE WITHOUT ID
>>     file.link AS date,
>>     theme
>> FROM #log/month
>> WHERE
>>     date-created >= this.date-created - dur(12 months)
>>     AND date-created < this.date-created
>> sort file.name ASC
>> LIMIT 10
>> ```
>> `INPUT[textArea(placeholder(theme)):theme]`
>
>> [!reflection]- ### highlights
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     weeks.highlights AS highlights
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> sort file.name ASC
>> LIMIT 10
>> ```
>> - `INPUT[textArea(placeholder('highlights : key moments? what excited me? how did I move things forward?')):highlights]`
>
>> [!reflection]- ### next-plans
>> ```dataview
>> TABLE WITHOUT ID
>>     weeks.file.link AS date, 
>>     weeks.next-plans AS next-plans
>> FROM #log/month
>> WHERE file.name = this.file.name
>> FLATTEN weeks
>> sort file.name ASC
>> LIMIT 10
>> ```
>> `INPUT[textArea(placeholder('next-plans : plan the next period, set goals, and any tasks to do')):next-plans]`
>
>> [!photo]-
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
>> LIMIT 10
>> ```
>> `BUTTON[add-banner-direct-image-URL]`
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
