---
up:
  - "[[Meta bind embeds (MOC, datascope)]]"
date-created: 2024-11-11
cssclasses: []
tags:
  - meta-bind-embed
---
```meta-bind-embed
[[Shortcuts widget - meta bind]]
```
> [!link|no icon]+ `=link(dateformat(date(this.file.day) - dur(1day), "yyyy-MM-dd"), "◀🌅")` `BUTTON[periodic-notes-date-switcher]` `=link([[Home]], "🏠")` `=this.file.aliases` `=link(dateformat(date(this.file.day) + dur(1day), "yyyy-MM-dd"), "🌅▶")`
> 
>> [!photo]- snapshot
>> - `=link(this.yesterday, "yesterday")`
>>     - sleep-comments : `=default(this.yesterday.sleep-comments, "*set yesterday's next-plans*")`
>>     - highlights : `=default(this.yesterday.highlights, "*set yesterday's highlights*")`
>>     - lessons-learned : `=default(this.yesterday.lessons-learned, "*set yesterday's lessons learned*")`
>>     - next-plans : `=default(this.yesterday.next-plans, "*set yesterday's next-plans*")`
>> - last week `=default(link(this.week.last-week), "Last week")` 
>>     - sleep-comments : `=default(this.week.last-week.sleep-comments, "*set last week's sleep-comments*")`
>>     - highlights : `=default(this.week.last-week.highlights, "*set last week's hightlights*")`
>>     - lessons-learned : `=default(this.week.last-week.lessons-learned, "*set last week's lessons-learned*")`
>>     - next-plans : `=default(this.week.last-week.next-plans, "*set last week's next-plans*")`
>> - `=default(link(dateformat(date(this.date-created), "yyyy-MM"), dateformat(date(this.date-created), "MMMM")), "this month")` %% monthly notes are referenced directly from this.date-created, rather than through this.week.month, due to month changes not overlapping with week changes %%
>>     - theme : **`=default(link(dateformat(date(this.date-created), "yyyy-MM")).theme, "*set this month's theme*")`**
>>     - next-plans (from last month) : `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).next-plans, "*set last month's next-plans*")`
>
>> [!morning]-
>> 
>> ### sleep 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(file.link, dateformat(file.day, "ccc dd")) AS date,
>>     default("**" + default(date(awake) - date(sleep), "_no sleep data!_") + "**<br>" +
>>         dateformat(date(sleep), "t") + " → " + 
>>         dateformat(date(awake), "t"), "test") AS sleep,
>>     sleep-comments AS "comments"
>> FROM #log/day
>> WHERE 
>>     file.day >= this.file.day - dur(7 days) 
>>     AND file.day < this.file.day
>> SORT file.day ASC
>> ```
>> - sleep duration today : **`=this.awake - this.sleep`** (`=dateformat(this.sleep, "t")` → `=dateformat(this.awake, "t")`) 
>>
>> `INPUT[textArea(placeholder('sleep comments : observations, patterns and reflections on sleep quality')):sleep-comments]`
>> 
>> ### gratitude
>> ```dataview
>> TABLE WITHOUT ID
>>     link(file.link, dateformat(file.day, "ccc dd")) AS date,
>>     gratitude
>> FROM #log/day
>> WHERE 
>>     gratitude 
>>     AND file.day >= this.file.day - dur(7 days) 
>>     AND file.day < this.file.day
>> SORT file.day ASC
>> ```
>> `INPUT[textArea(placeholder('gratitude : what are 3 to 10 things that I am grateful for? what is something that I have never noticed in my immediate environment?')):gratitude]`
>
>> [!evening]-
>>
>> ### highlights
>> ```dataview
>> TABLE WITHOUT ID
>>     link(file.link, dateformat(file.day, "ccc dd")) AS date,
>>     highlights
>> FROM #log/day
>> WHERE 
>>     file.day >= this.file.day - dur(7 days) 
>>     AND file.day < this.file.day
>> SORT file.day ASC
>> ```
>> - `="[select Google photo](" + this.url + ")"` `BUTTON[add-banner-Google-Photo-share-link]`
>>
>> `INPUT[textArea(placeholder('highlights : key moments? what excited me? how did I move things forward?')):highlights]`
>> 
>> ### lessons-learned
>> ```dataview
>> TABLE WITHOUT ID
>>     link(file.link, dateformat(file.day, "ccc dd")) AS date,
>>     lessons-learned AS "lessons learned"
>> FROM #log/day
>> WHERE 
>>     file.day >= this.file.day - dur(7 days) 
>>     AND file.day < this.file.day
>> SORT file.day ASC
>> ```
>> `INPUT[textArea(placeholder('lessons learned : insights, mistakes, failures and a functional interpretation')):lessons-learned]`
>> 
>> ### next-plans 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(file.link, dateformat(file.day, "ccc dd")) AS date,
>>     next-plans AS "next-plans"
>> FROM #log/day
>> WHERE sleep-comments OR (sleep AND awake) OR gratitude OR intentions OR highlights OR lessons-learned OR next-plans
>> WHERE 
>>     file.day >= this.file.day - dur(7 days) 
>>     AND file.day < this.file.day
>> SORT file.day ASC
>> ```
>> `INPUT[textArea(placeholder('next-plans : plan the next period, set goals, and any tasks to do')):next-plans]`
>
>> [!search|c]- periodic notes support pages
>> ```meta-bind-embedzcancel
>> [[Periodic notes support pages - meta bind]]
>> ```
>
> [[Periodic notes daily note - meta bind embed|📝]] `BUTTON[morning-pages]`

```meta-bind-embed
[[Meta bind button index - Meta bind embed]]
```
