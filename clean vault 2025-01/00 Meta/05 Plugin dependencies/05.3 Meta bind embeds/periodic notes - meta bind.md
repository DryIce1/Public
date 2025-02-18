---
up:
  - "[[Meta bind embeds (MOC, datascope)]]"
date-created: 2024-11-11
cssclasses:
  - log
tags:
  - meta-bind-embed
dateBind: 2025-01-16
---

> [!NOTE] Update the dateBind in order to update the reference date for the callout below


> [!link|no icon]- `=link(dateformat(date(link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day) - dur(1day), "yyyy-MM-dd"), "◀🌅")` `BUTTON[periodic-notes-date-switcher]` `=link([[Home]], "🏠")` `=link(dateformat(this.dateBind, "yyyy-MM-dd")).file.aliases` `=link(dateformat(date(link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day) + dur(1day), "yyyy-MM-dd"), "🌅▶")`
> 
>> [!photo]- snapshot
>> - `=link(link(dateformat(this.dateBind, "yyyy-MM-dd")).yesterday, "yesterday")`
>>     - sleep-comments : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).yesterday.sleep-comments, "*set yesterday's next-plans*")`
>>     - highlights : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).yesterday.highlights, "*set yesterday's highlights*")`
>>     - lessons learned : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).yesterday.lessons-learned, "*set yesterday's lessons learned*")`
>>     - next-plans : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).yesterday.next-plans, "*set yesterday's next-plans*")`
>> - `=default(link(link(dateformat(this.dateBind, "yyyy-MM-dd")).week.last-week, "last week"), "Last week")` 
>>     - sleep-comments : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).week.last-week.sleep-comments, "*set this week's sleep-comments*")`
>>     - highlights : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).week.last-week.highlights, "*set this week's hightlights*")`
>>     - lessons-learned : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).week.last-week.lessons-learned, "*set this week's lessons-learned*")`
>>     - next-plans : `=default(link(dateformat(this.dateBind, "yyyy-MM-dd")).week.last-week.next-plans, "*set this week's next-plans*")`
>> - `=default(link(dateformat(date(link(dateformat(this.dateBind, "yyyy-MM-dd")).date-created), "yyyy-MM"), dateformat(date(link(dateformat(this.dateBind, "yyyy-MM-dd")).date-created), "MMMM")), "this month")` %% monthly notes are referenced directly from link(dateformat(this.dateBind, "yyyy-MM-dd")).date-created, rather than through link(dateformat(this.dateBind, "yyyy-MM-dd")).week.month, due to month changes not overlapping with week changes %%
>>     - theme : **`=default(link(dateformat(date(link(dateformat(this.dateBind, "yyyy-MM-dd")).date-created), "yyyy-MM")).theme, "*set this month's theme*")`**
>>     - next-plans (from last month) : `=default(link(dateformat(date(link(dateformat(this.dateBind, "yyyy-MM-dd")).date-created - dur(1 month)), "yyyy-MM")).next-plans, "*set last month's next-plans*")`
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
>>     file.day >= link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day - dur(7 days) 
>>     AND file.day < link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day
>> SORT file.day ASC
>> ```
>> - sleep duration today : **`=link(dateformat(this.dateBind, "yyyy-MM-dd")).awake - link(dateformat(this.dateBind, "yyyy-MM-dd")).sleep`** (`=dateformat(link(dateformat(this.dateBind, "yyyy-MM-dd")).sleep, "t")` → `=dateformat(link(dateformat(this.dateBind, "yyyy-MM-dd")).awake, "t")`) 
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
>>     AND file.day >= link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day - dur(7 days) 
>>     AND file.day < link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day
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
>>     file.day >= link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day - dur(7 days) 
>>     AND file.day < link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day
>> SORT file.day ASC
>> ```
>> - `="[select Google photo](" + link(dateformat(this.dateBind, "yyyy-MM-dd")).url + ")"` `BUTTON[add-banner-Google-Photo-share-link]`
>>
>> `INPUT[textArea(placeholder('highlights : key moments? what excited me? how did I move things forward?')):highlights]`
>> 
>> ### lessons learned
>> ```dataview
>> TABLE WITHOUT ID
>>     link(file.link, dateformat(file.day, "ccc dd")) AS date,
>>     lessons-learned AS "lessons learned"
>> FROM #log/day
>> WHERE 
>>     file.day >= link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day - dur(7 days) 
>>     AND file.day < link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day
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
>>     file.day >= link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day - dur(7 days) 
>>     AND file.day < link(dateformat(this.dateBind, "yyyy-MM-dd")).file.day
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

```meta-bind-button
label: Create morning pages section
icon: ""
hidden: true
class: ""
tooltip: Insert Morning Pages section
id: morning-pages
style: default
action:
  type: "regexpReplaceInNote"
  regexp: "^# Log"
  regexpFlags: "gm"
  replacement: "\n# Morning Pages\n\n\n# Log"
```

```meta-bind-button
label: ""
icon: calendar-search
hidden: true
class: ""
tooltip: Periodic notes date switcher
id: periodic-notes-date-switcher
style: default
actions:
  - type: command
    command: periodic-notes:show-date-switcher

```

```meta-bind-button
label: ""
icon: image
hidden: true
class: ""
tooltip: paste Google Photo share link into banner YAML property
id: add-banner-Google-Photo-share-link
style: default
actions:
  - type: command
    command: templater-obsidian:00 Meta/05 Plugin dependencies/05.1 Templates/JS templates/Google photo share link, paste to banner property - JS template.md
```

