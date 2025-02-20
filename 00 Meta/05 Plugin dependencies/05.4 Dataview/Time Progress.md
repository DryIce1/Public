---
aliases: 
cssClass: 
date-created: 2023-01-01
publish: false
tags:
  - atlas/core
---
%% For the life progress bar, be sure to update date of birth %%
```dataviewjs
const today = DateTime.now()
const endOfYear = {
    year: today.year,
    month: 12,
    day: 31
}
const lifespan = { year: 80 } 
const birthday = DateTime.fromObject({
    year: 1988,
    month: 11,
    day: 5
});
const deathday = birthday.plus(lifespan)
function progress(type) {
    let value;
    
    switch(type) {
        case "lifespan": 
            value = (today.year - birthday.year) / lifespan.year * 100;
            break;
        case "year":
            value = today.month / 12 * 100
            break;
        case "month":
            value = today.day / today.daysInMonth * 100
            break;
        case "day":
            value = today.hour / 24 * 100
            break;
    }
    return `<progress value="${parseInt(value)}" max="100"></progress> | ${parseInt(value)} %`
}
dv.span(`
|  | Progress  | % |
| --- |--- |---|
| **Year** | ${progress("year")}
| **Month**| ${progress("month")}
| **Day**| ${progress("day")}
| **Life** | ${progress("lifespan")}
`)
```
