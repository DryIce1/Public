---
up:
  - "[[Meta bind embeds (MOC, datascope)]]"
date-created: 2024-11-11
cssclasses:
  - log
tags:
  - meta-bind-embed
---
> [!link|no icon]- `= link(dateformat(date(this.days[3]) - dur(1 week), "yyyy-'W'WW"), "◀7")` `BUTTON[periodic-notes-date-switcher]` `= split(this.file.name, "-")[1] + " : " + dateformat(date(this.days[0]), "DDDD")` to `= dateformat(date(this.days[6]), "DDDD")` `= link(dateformat(date(this.days[3]) + dur(1 week), "yyyy-'W'WW"), "▶️7")`
>
>> [!photo]- snapshot
>> - last week `=default(link(this.last-week), "Last week")` 
>>     - sleep-comments : `=default(this.last-week.next-plans, "**set last week's sleep-comments**")`
>>     - highlights : `=default(this.last-week.next-plans, "**set last week's highlights**")`
>>     - lessons-learned : `=default(this.last-week.next-plans, "**set last week's next-plans**")`
>>     - next-plans : `=default(this.last-week.next-plans, "**set last week's next-plans**")`
>> - `=default(link(dateformat(date(this.date-created), "yyyy-MM"), dateformat(date(this.date-created), "MMMM")), "this month")` %% this month %%
>>     - theme : `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).theme, "**set this month's theme**")`
>> - `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM"), dateformat(date(this.date-created - dur(1 month)), "MMMM")), "**create last month's note**")` %% last month %%
>>     - theme : `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).theme, "**set last month's theme**")`
>>     - sleep-comments : `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).sleep-comments, "**set last month's sleep-comments**")`
>>     - highlights : `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).highlights, "**set last month's highlights**")`
>>     - lessons-learned : `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).lessons-learned, "**set last month's lessons-learned**")`
>>     - next-plans : `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).next-plans, "**set last month's next-plans**")`
>> 
>>> [!image-description] `=choice(this.banner, "![photo|300](" + this.banner + ")" + choice(this.banner-description, this.banner-description, ""), "**copy photo and paste into banner button below**")`
>
>> [!reflection]- ### sleep
>>```dataview
>> TABLE WITHOUT ID
>>     link(days.file.link, dateformat(days.file.day, "ccc dd ")) AS date,
>>     "**"+(days.awake - days.sleep)  + "**<br>" +
>>         dateformat(days.sleep, "t") + " → " + dateformat(days.awake, "t")  as sleep,
>>     days.sleep-comments as "sleep-comments"
>> FROM #log/week
>> WHERE file.name = this.file.name
>> FLATTEN days
>> LIMIT 10
>> ```
>> 
>>```dataviewjs
>> // Get the current file's metadata, including the list of days
>> const days = dv.current().days;
>> 
>> if (!days || days.length === 0) {
>>     dv.paragraph("No days available to calculate sleep duration.");
>> } else {
>>     let totalSleep = 0;
>>     let validDays = 0;
>> 
>>     for (let day of days) {
>>         // Load each linked day note and extract its metadata
>>         const dayFile = dv.page(day);
>> 
>>         if (dayFile?.sleep && dayFile?.awake) {
>>             const sleepTime = new Date(dayFile.sleep);
>>             const awakeTime = new Date(dayFile.awake);
>> 
>>             if (!isNaN(sleepTime) && !isNaN(awakeTime)) {
>>                 // Calculate the sleep duration in hours
>>                 const sleepDuration = (awakeTime - sleepTime) / (1000 * 60 * 60);
>>                 totalSleep += sleepDuration;
>>                 validDays++;
>>             }
>>         }
>>     }
>> 
>>     // Display average sleep duration if valid days exist
>>     if (validDays >> 0) {
>>         const avgSleep = totalSleep / validDays;
>>         const hours = Math.floor(avgSleep);
>>         const minutes = Math.round((avgSleep - hours) * 60);
>>         const avgSleepRounded = parseFloat(avgSleep.toFixed(2))
>>         dv.paragraph(`Calculated average sleep duration: **${hours} hours, ${minutes} minutes** (${avgSleepRounded})`);
>> 
>>     } else {
>>         dv.paragraph("No valid sleep data available for this week.");
>>     }
>> }
>> ```
>> 
>> insert sleep duration : `INPUT[number():sleep-duration]`
>> 
>> `INPUT[textArea(placeholder('sleep comments : observations, patterns and reflections on sleep quality')):sleep-comments]`
>
>> [!reflection]- ### gratitude
>>```dataview
>> TABLE WITHOUT ID
>> link(days.file.link, dateformat(days.file.day, "ccc dd ")) AS date,
>>     days.gratitude AS gratitude
>> FROM #log/week
>> WHERE file.name = this.file.name
>> FLATTEN days
>> LIMIT 10
>> ```
>> `INPUT[textArea(placeholder('gratitude : what are 3 to 10 things that I am grateful for? what is something that I have never noticed in my immediate environment?')):gratitude]`
>
>> [!reflection]- ### highlights
>>```dataview
>> TABLE WITHOUT ID
>> link(days.file.link, dateformat(days.file.day, "ccc dd ")) AS date,
>>     days.highlights AS highlights
>> FROM #log/week
>> WHERE file.name = this.file.name
>> FLATTEN days
>> LIMIT 10
>> ```
>> - `INPUT[textArea(placeholder('highlights : key moments? what excited me? how did I move things forward?')):Highlights]`
>
>> [!reflection]- ### lessons-learned
>>```dataview
>> TABLE WITHOUT ID
>> link(days.file.link, dateformat(days.file.day, "ccc dd ")) AS date,
>>     days.lessons-learned AS lessons-learned
>> FROM #log/week
>> WHERE file.name = this.file.name
>> FLATTEN days
>> LIMIT 10
>> ```
>> `INPUT[textArea(placeholder('lessons learned : insights, mistakes, failures and a functional interpretation')):lessons-learned]`
>
>> [!reflection]- ### next-plans
>>```dataview
>> TABLE WITHOUT ID
>> link(days.file.link, dateformat(days.file.day, "ccc dd ")) AS date,
>>     days.next-plans AS next-plans
>> FROM #log/week
>> WHERE file.name = this.file.name
>> FLATTEN days
>> LIMIT 10
>> ```
>> `INPUT[textArea(placeholder('next-plans : plan the next period, set goals, and any tasks to do')):next-plans]`
>
>> [!photo]-
>> 
>> ```dataview
>> TABLE WITHOUT ID
>>     link(days.file.link, dateformat(days.file.day, "ccc dd ")) AS date, 
>>     choice(days.banner, "<br><sub><small>" + "[URL](" +  days.banner + ")" + "</small></sub>" +
>>     "![photo](" + days.banner + ")", "") 
>>     AS ""
>> FROM #log/week
>> WHERE file.name = this.file.name
>> FLATTEN days
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
> - [[Periodic notes weekly note - meta bind embed|📝 edit embed]] | [[Periodic notes - checklist - weekly|✔️edit review checklist]] | `BUTTON[weekly-review]`

# Daily note logs

> [!NOTE]- daily note logs
>
>> [!log]+ ## `=link(dateformat(date(this.days[0].file.name), "yyyy-MM-dd"), dateformat(date(this.days[0].file.name), "DDDD"))`
>> `="![[" + this.days[0].file.name + "#log]]"`
>
>> [!log]+ ## `=link(dateformat(date(this.days[1].file.name), "yyyy-MM-dd"), dateformat(date(this.days[1].file.name), "DDDD"))`
>> `="![[" + this.days[1].file.name + "#log]]"`
>
>> [!log]+ ## `=link(dateformat(date(this.days[2].file.name), "yyyy-MM-dd"), dateformat(date(this.days[2].file.name), "DDDD"))`
>> `="![[" + this.days[2].file.name + "#log]]"`
>
>> [!log]+ ## `=link(dateformat(date(this.days[3].file.name), "yyyy-MM-dd"), dateformat(date(this.days[3].file.name), "DDDD"))`
>> `="![[" + this.days[3].file.name + "#log]]"`
>
>> [!log]+ ## `=link(dateformat(date(this.days[4].file.name), "yyyy-MM-dd"), dateformat(date(this.days[4].file.name), "DDDD"))`
>> `="![[" + this.days[4].file.name + "#log]]"`
>
>> [!log]+ ## `=link(dateformat(date(this.days[5].file.name), "yyyy-MM-dd"), dateformat(date(this.days[5].file.name), "DDDD"))`
>> `="![[" + this.days[5].file.name + "#log]]"`
>
>> [!log]+ ## `=link(dateformat(date(this.days[6].file.name), "yyyy-MM-dd"), dateformat(date(this.days[6].file.name), "DDDD"))`
>> `="![[" + this.days[6].file.name + "#log]]"`

```meta-bind-embed
[[Meta bind button index - Meta bind embed]]
```
