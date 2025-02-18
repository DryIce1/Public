<%"---"%>
last-month: 
- "[[<% tp.date.now("YYYY-MM", -15, tp.file.title, "YYYY-MM") %>]]"
quarter: 
  - "[[<% tp.date.now("YYYY-[Q]Q", +0, tp.file.title, "YYYY-MM") %>]]"
year: 
  - "[[<% tp.date.now("YYYY", +0, tp.file.title, "YYYY-MM") %>]]"
weeks:<%*
const monthMoment = moment(tp.file.title, "YYYY-MM");
const monthStart = monthMoment.clone().startOf('month');
const monthEnd = monthMoment.clone().endOf('month');
const currentMonth = monthMoment.month();

let weekStart = monthStart.clone().startOf('isoWeek');

while (weekStart.isBefore(monthEnd.clone().add(1, 'day'))) {
  if (weekStart.month() === currentMonth) {
    tR += `\n- "[[${weekStart.format("YYYY-[W]WW")}]]"`;
  }
  weekStart.add(1, 'week');
}
%>
date-created: <% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-MM") %>
aliases: <% tp.date.now("MMMM YYYY", +0, tp.file.title, "YYYY-MM") %>
tags: log/month
cssClasses:
  - log
publish: false
banner: 
banner-y: 
cover: 
<%"---"%>

```meta-bind-embed
[[Meta bind monthly note embed]]
```

# Log<%tp.file.cursor(0)%>