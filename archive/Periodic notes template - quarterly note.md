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
<%"---"%>

##### linked items
Accomplishments↗:: 
What Worked?🔗:: 
Other Highs:: 
Disappointments↗:: 
Lessons Learned🔗:: 
Other Lows::
Active Project Count🔗::
Goal Outcomes↗:: 
Projects↗:: 
Year↗:: 

----
## 1. Reflection
-   [ ] Review **Accomplishments** and **Disappointments**
    %% [[Pasted image 20210829215523.png]] [[Pasted image 20210829215554.png]] [[Pasted image 20210829215603.png]] %%

### What's Working?
- 

### What's Not Working?
- 

### Changes to Plan or Approach?
- 

## 2. Process & Update
-   [ ] Review Quarters - On track?
    -   **View Quarters**
    %% see tables on notion for all columns [[Pasted image 20210829220034.png]] %%
-   [ ] Review/Update Quarter Assignments for Outcome Goals and Projects
    -   **View Goal Outcomes & Projects**
    %%  see tables on notion for all columns  [[Pasted image 20210829220440.png]] %%

# 3. **Someday/Maybe Items**
-   [ ] Review. Any ready to activate?
    -   **View Someday/Maybe Projects**