<%"---"%>
months: <%*
const quarterMoment = moment(tp.file.title, "YYYY-[Q]Q");
if (!quarterMoment.isValid()) {
  tR += "Invalid quarter date format. Expected 'YYYY-Qn'.";
} else {
  const monthsInQuarter = [];
  const startMonth = (quarterMoment.quarter() - 1) * 3; // Quarters start at 1
  for (let i = 0; i < 3; i++) {
    const monthMoment = quarterMoment.clone().month(startMonth + i);
    monthsInQuarter.push(`\n  - "[[${monthMoment.format("YYYY-MM")}]]"`);
  }
  tR += monthsInQuarter.join("");
}
%>
year: [[<% tp.date.now("YYYY", +0, tp.file.title, "YYYY-MM") %>]]
tags: log/quarterly
type: log/quarterly
<%"---"%>

```meta-bind-embed
[[Periodic notes quarterly note - meta bind embed]]
```

## Reflection
-   [ ] Review **Accomplishments** and **Disappointments**
### What's Working?
- 

### What's Not Working?
- 

### Changes to Plan or Approach?
- 

## 2. Process & Update
-   [ ] Review Quarters - On track?
    -   **View Quarters**
-   [ ] Review/Update Quarter Assignments for Outcome Goals and Projects
    -   **View Goal Outcomes & Projects**

# 3. **Someday/Maybe Items**
-   [ ] Review. Any ready to activate?
    -   **View Someday/Maybe Projects**