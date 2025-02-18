---
up: 
related: 
aliases: 
tags: 
banner:
cover: 
publish: 
date-created: 2023-02-05
type: 
status: 
area: 
topic: 
summary: 
---
# Templater
>[!NOTE] reminders for the tp.date.now function 
>     - first part - output format 
>     - second part - offset period
>     - third part - input format based on note title 

- Days 
    - **Today**[[<% tp.date.now("YYYY-MM-DD", 0, tp.file.title, "YYYY-MM-DD") %>]] 
    - **yesterday** [[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>]] 
    - **tomorrow** [[<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>]]
- Weeks 
    - **this week** [[<% tp.date.now("YYYY-[W]WW", "P-1W", tp.file.title, "YYYY-[W]WW") %>]] 
    - **previous week** [[<% tp.date.now("YYYY-[W]WW", "P-1W", tp.file.title, "YYYY-[W]WW") %>]]
    - **next week** [[<% tp.date.now("YYYY-[W]WW", "P1W", tp.file.title, "YYYY-[W]WW") %>]] 
- Months 
    - **this month** [[<% tp.date.now("YYYY-MM", "", tp.file.title, "YYYY-MM") %>]]
    - **last month** [[<% tp.date.now("YYYY-MM", "P-1M", tp.file.title, "YYYY-MM") %>]]
    - **next month** [[<% tp.date.now("YYYY-MM", "P1M", tp.file.title, "YYYY-MM") %>]]
- Years 
    - **this year** [[<% tp.date.now("YYYY", "", tp.file.title, "YYYY") %>]]
    - **last year** [[<% tp.date.now("YYYY", "P-1Y", tp.file.title, "YYYY") %>]]
    - **next year** [[<% tp.date.now("YYYY", "P1Y", tp.file.title, "YYYY") %>]] 

# Periodic notes navigation

>[!NOTE] Use this reference to make links and view properties relative to any note's date-created YYYY-MM-DD date

### Daily notes (format: `YYYY-MM-DD`)

- Navigation
    - today:  
        `=link(dateformat(date(this.date-created), "yyyy-MM-dd"), "🌤️")`  
        `=link(dateformat(date(this.date-created), "yyyy-MM-dd"), "")`  
        `=link(dateformat(date(this.date-created), "yyyy-MM-dd"), dateformat(date(this.date-created), "DD"))`  
        `=link(dateformat(date(this.date-created), "yyyy-MM-dd"), dateformat(date(this.date-created), "DDD"))`  
        `=link(dateformat(date(this.date-created), "yyyy-MM-dd"), dateformat(date(this.date-created), "DDDD"))`
    - yesterday:  
        `=link(dateformat(date(this.date-created) - dur(1 day), "yyyy-MM-dd"), "◀🌤️")`
    - tomorrow:  
        `=link(dateformat(date(this.date-created) + dur(1 day), "yyyy-MM-dd"), "🌤️▶")`
- Properties
    - yesterday  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).yesterday`
    - week  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).week`
        - week, last week  
            `=link(dateformat(this.date-created, "yyyy-MM-dd")).week.last-week`
            - week, last week, days  
                `=link(dateformat(this.date-created, "yyyy-MM-dd")).week.last-week.days`
            - week, last week, first day of the week  
                `=link(dateformat(this.date-created, "yyyy-MM-dd")).week[0].last-week[0].days[0]`
    - aliases  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).aliases`
        - aliases link  
            `=link(dateformat(this.date-created, "yyyy-MM-dd"), link(dateformat(this.date-created, "yyyy-MM-dd"), "").aliases)`
    - banner  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).banner`
    - sleep  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).sleep`
    - awake  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).awake`
    - sleep-comments  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).sleep-comments`
    - gratitude  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).gratitude`
    - intentions  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).intentions`
    - highlights  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).highlights`
    - lessons-learned  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).lessons-learned`
    - next-plans  
        `=link(dateformat(this.date-created, "yyyy-MM-dd")).next-plans`

### Weekly notes (format: `YYYY-'W'WW`)
- Navigation
    - This Week:  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW"), "")`  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW"), "7️⃣")`
    - Last Week:  
        `=link(dateformat(date(this.date-created) - dur(1 week), "yyyy-'W'WW"), "◀7️⃣")`
    - Next Week:  
        `=link(dateformat(date(this.date-created) + dur(1 week), "yyyy-'W'WW"), "7️⃣▶")`
- Properties
    - last-week  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).last-week`
    - month  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).month`
    - days  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).days`
        - first day of the week  
            `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).days[0]`
    - aliases  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).aliases`
        - aliases link  
            `=link(dateformat(this.date-created, "yyyy-'W'WW"), link(dateformat(this.date-created, "yyyy-'W'WW"), "").aliases)`
    - banner  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).banner`
    - sleep-comments  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).sleep-comments`
    - gratitude  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).gratitude`
    - intentions  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).intentions`
    - highlights  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).highlights`
    - lessons-learned  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).lessons-learned`
    - next-plans  
        `=link(dateformat(date(this.date-created), "yyyy-'W'WW")).next-plans`

### Monthly notes (format: `YYYY-MM`)
- Navigation
    - This Month:  
        `=link(dateformat(date(this.date-created), "yyyy-MM"), "")`  
        `=link(dateformat(date(this.date-created), "yyyy-MM"), dateformat(date(this.date-created), "yyyy-MMMM"))`  
        `=link(dateformat(date(this.date-created), "yyyy-MM"), "📅")`
    - Last Month:  
        `=link(dateformat(date(this.date-created) - dur(1 month), "yyyy-MM"), "◀📅")`
        `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM"), dateformat(date(this.date-created - dur(1 month)), "MMMM")), "**create last month's note**")`
    - Next Month:  
        `=link(dateformat(date(this.date-created) + dur(1 month), "yyyy-MM"), "📅▶")`
- Properties
    - last-month  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).last-month`
    - quarter  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).quarter`
    - year  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).year`
    - weeks  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).weeks`
    - aliases  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).aliases`
        - aliases link  
            `=link(dateformat(date(this.date-created), "yyyy-MM"), link(dateformat(date(this.date-created), "yyyy-MM")).aliases)`
    - banner  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).banner`
    - theme  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).theme`
        `=default(link(dateformat(date(this.date-created), "yyyy-MM")).theme, "**set this month's theme**")`
    - sleep-comments  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).sleep-comments`
    - gratitude  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).gratitude`
    - intentions  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).intentions`
    - highlights  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).highlights`
    - lessons-learned  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).lessons-learned`
    - next-plans  
        `=link(dateformat(date(this.date-created), "yyyy-MM")).next-plans`
        - date created last-month's next-plans, default to "set last-month's next-plans" : 
            `=default(link(dateformat(date(this.date-created - dur(1 month)), "yyyy-MM")).next-plans, "**set this month's next-plans**")`

### Quarterly notes (format: `YYYY-'Q'q`)
- Navigation
    - This Quarter:  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q"), "🧭")`  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q"), "")`  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q"), dateformat(date(this.date-created), "yyyy - 'Quarter 'q"))`
    - Last Quarter:  
        `=link(dateformat(date(this.date-created) - dur(3 months), "yyyy-'Q'q"), "◀🧭")`
    - Next Quarter:  
        `=link(dateformat(date(this.date-created) + dur(3 months), "yyyy-'Q'q"), "🧭▶")`
- Properties
    - last-quarter  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q")).last-quarter`
    - year  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q")).year`
    - months  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q")).months`
    - aliases  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q")).aliases`
        - aliases link  
            `=link(dateformat(date(this.date-created), "yyyy-'Q'q"), link(dateformat(date(this.date-created), "yyyy-'Q'q")).aliases)`
    - banner  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q")).banner`
    - theme  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q")).theme`
    - projects  
        `=link(dateformat(date(this.date-created), "yyyy-'Q'q")).projects`

### Yearly notes (format: `YYYY`)
- Navigation
    - This Year:  
        `=link(dateformat(date(this.date-created), "yyyy"), "📖")`  
        `=link(dateformat(date(this.date-created), "yyyy"), "")`
    - Back:  
        `=link(dateformat(date(this.date-created) - dur(1 year), "yyyy"), "◀📖")`
    - Forwards:  
        `=link(dateformat(date(this.date-created) + dur(1 year), "yyyy"), "📖▶")`
- Properties
    - last-year  
        `=link(dateformat(date(this.date-created), "yyyy")).last-year`
    - quarters  
        `=link(dateformat(date(this.date-created), "yyyy")).quarters`
    - months  
        `=link(dateformat(date(this.date-created), "yyyy")).months`
    - decade  
        `=link(dateformat(date(this.date-created), "yyyy")).decade`
    - aliases  
        `=link(dateformat(date(this.date-created), "yyyy")).aliases`
        - aliases file link  
            `=link(dateformat(date(this.date-created), "yyyy"), link(dateformat(date(this.date-created), "yyyy")).aliases)`
    - banner  
        `=link(dateformat(date(this.date-created), "yyyy")).banner`
    - theme  
        `=link(dateformat(date(this.date-created), "yyyy")).theme`
    - sleep-comments  
        `=link(dateformat(date(this.date-created), "yyyy")).sleep-comments`
    - gratitude  
        `=link(dateformat(date(this.date-created), "yyyy")).gratitude`
    - intentions  
        `=link(dateformat(date(this.date-created), "yyyy")).intentions`
    - highlights  
        `=link(dateformat(date(this.date-created), "yyyy")).highlights`
    - lessons-learned  
        `=link(dateformat(date(this.date-created), "yyyy")).lessons-learned`
    - next-plans  
        `=link(dateformat(date(this.date-created), "yyyy")).next-plans`

### Decade notes (format: `YYYY-YYYY`)
- Navigation
    - This Decade:  
        `=link((floor(date(this.date-created).year / 10) * 10) + "-" + (floor(date(this.date-created).year / 10) * 10 + 9), "")`  
        `=link((floor(date(this.date-created).year / 10) * 10) + "-" + (floor(date(this.date-created).year / 10) * 10 + 9), "🕰️")`
    - Back:  
        `=link((floor(date(this.date-created).year / 10) - 1) * 10 + "-" + ((floor(date(this.date-created).year / 10) - 1) * 10 + 9), "◀🕰️")`
    - Forwards:  
        `=link((floor(date(this.date-created).year / 10) + 1) * 10 + "-" + ((floor(date(this.date-created).year / 10) + 1) * 10 + 9), "🕰️▶")`
- Properties
    - last-decade  
        `=link((floor(date(this.date-created).year / 10) * 10) + "-" + (floor(date(this.date-created).year / 10) * 10 + 9), "").last-decade`

## Other snippets
- Ordinal (day of year)
    `=dateformat(date(today), "o")` of 365
- Week number  
    `=dateformat(date(today), "W")`
- Day of the week  
    `=dateformat(date(today), "EEEE")`


# Dataview
- Weekly note dataview navigation etc
    - `= dateformat(date(this.days[3]) - dur(1 week), "yyyy-MM-dd")`
    - `= dateformat(date(this.days[3]) - dur(1 week), "yyyy-'W'WW")`
    - `= dateformat(date(this.days[3]) + dur(1 week), "yyyy-'W'WW")`
    - link to start of last week : `= link(dateformat(date(this.days[0]) - dur(1 week), "yyyy-MM-dd"), dateformat(date(this.days[0]) - dur(1 week), "◀7"))` 
    - title callout for weekly note : `= dateformat(date(this.days[0]), "DDDD")` to `= dateformat(date(this.days[6]), "DDDD")` 