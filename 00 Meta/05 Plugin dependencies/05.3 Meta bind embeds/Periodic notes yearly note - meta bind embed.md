---
up:
  - "[[Meta bind embeds (MOC, datascope)]]"
date-created: 2024-11-11
cssclasses:
  - log
tags:
  - meta-bind-embed
---

> [!link|no icon]- `=link(dateformat(this.date-created - dur(1 year), "yyyy"), dateformat(this.date-created - dur(1 year), "◀️ yyyy"))` `BUTTON[periodic-notes-date-switcher]` `= this.file.name + choice(this.aliases, " : **" + this.aliases + "**",  "")` `=link(dateformat(this.date-created + dur(1 year), "yyyy"), dateformat(this.date-created + dur(1 year), "yyyy ▶️"))`
> 
> ## reflections
> 
>> [!photo]+ snapshot
>> - `=this.year` 
>>     - theme : `=default(this.theme, "**enter theme below**")`
>>     - intentions : `=default(this.intentions, "**enter intentions below**")`
>> - `=link(this.last-year)` 
>>     - theme : `=this.last-year.theme`
>>     - highlights : `=this.last-year.highlights`
>>     - intentions : `=this.last-year.intentions`
>> - `=this.decade`
>>     - theme : `=this.decade.theme` 
>>     - intentions : `=this.decade.intentions`
>> - theme for this year `=dateformat(this.date-created, "yyyy")` : `INPUT[textArea(placeholder(theme)):theme]`
>
>> [!reflection]- ### sleep
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     "**" + months.sleep-duration + "** hours" AS "TotalSleepTime",
>>     months.sleep-comments AS "sleep comments"
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> sort file.name ASC
>> LIMIT 20
>> ```
>>
>> `INPUT[textArea(placeholder('sleep comments : observations, patterns and reflections on sleep quality')):sleep-comments]`
>> - average sleep duration : `=round(sum(nonnull(this.months.sleep-duration)) / length(nonnull(this.months.sleep-duration)), 2)` hours  `INPUT[number(placeholder(sleep duration)):sleep-duration]`
>
>> [!reflection]- ### gratitude
>> ```dataview
>> TABLE 
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.gratitude AS gratitude
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> sort file.name ASC
>> LIMIT 20
>> ```
>> `INPUT[textArea(placeholder('gratitude : what are 3 to 10 things that I am grateful for? what is something that I have never noticed in my immediate environment?')):gratitude]`
>
>> [!reflection]- ### intentions
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.intentions AS intentions
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> sort file.name ASC
>> LIMIT 20
>> ```
>> `INPUT[textArea(placeholder('intentions : what goals or mindsets for this period?')):intentions]`
>
>> [!reflection]- ### highlights
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.highlights AS highlights
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> sort file.name ASC
>> LIMIT 20
>> ```
>> - highlights : `INPUT[textArea(placeholder('highlights : key moments? what excited me? how did I move things forward?')):Highlights]`
>
>> [!reflection]- ### lessons-learned
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.lessons-learned AS lessons-learned
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> sort file.name ASC
>> LIMIT 20
>> ```
>> `INPUT[textArea(placeholder('lessons learned : insights, mistakes, failures and a functional interpretation')):lessons-learned]`
>
>> [!reflection]- ### next-plans
>> ```dataview
>> TABLE WITHOUT ID
>>     link(months.file.link, dateformat(months.date-created, "MMMM yy")) AS date, 
>>     months.next-plans AS next-plans
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> sort file.name ASC
>> LIMIT 20
>> ```
>> `INPUT[textArea(placeholder('next-plans : plan the next period, set goals, and any tasks to do')):next-plans]`
>
> ## Photo log
>> [!photo]+
>> ```dataview
>> TABLE WITHOUT ID
>>     "![photo](" + months.banner + ")" + "" +
>>     string(
>>         "<div style='text-align: center;'><sub><small>" +
>>         link(months.file.link, dateformat(months.date-created, "MMMM yyyy")) + 
>>         choice(
>>             nonnull(months.banner),
>>             " : " + "[*" + dateformat(months.date-created, "MMMM") + " photo URL*](" + banner + ")</small></sub>",
>>             ""
>>         ) +
>>         "</div>"
>>     ) AS ""
>> FROM #log/year
>> WHERE file.name = this.file.name
>> FLATTEN months
>> LIMIT 20
>> ```
>>
>> [!navigation]- 
>> ```meta-bind-embed
>> [[Meta bind periodic notes navigation embed]]
>> ```
> 
>> [!reflections]- properties at a glance
>>
>> ```meta-bind-embed
>> [[Properties at a Glance]]
>> ```
>
> [[Periodic notes yearly note - meta bind embed||📝 edit embed]] | [[Periodic notes - checklist - yearly - last year|✔️edit last-year's review checklist]] | [[Periodic notes - checklist - yearly - next year|✔️ edit next year's review checklist]] | `BUTTON[yearly-review-last-year]` | `BUTTON[yearly-review-next-year]`

```meta-bind-embed
[[Meta bind button index - Meta bind embed]]
```
