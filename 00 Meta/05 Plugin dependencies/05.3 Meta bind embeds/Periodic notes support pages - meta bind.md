---
up: 
related:
  - "[[Periodic notes navigation - meta bind embed]]"
aliases: 
tags: 
banner: 
cover: 
publish: false
date-created: 2024-11-25
type: 
area: 
topic: 
summary: 
---

> [!check]- Incomplete tasks from this month 
> %% this dataview query only works when accessed from a daily note or via a meta-bind embed %%
> ```dataview
> TASK 
> FROM #log/day 
> WHERE 
>     !completed AND !checked
>     AND week.month = this.week.month 
>     OR week.month = this.week.month.last-month[0]
> ```

> [!reflections]- properties at a glance
>
> ```meta-bind-embed
> [[Dataview - advanced inline queries at a glance]]
> ```

> [!tip]- Journaling Prompts
> ![[Journaling prompts]]

> [!NOTE]- periodic notes navigation
> ```meta-bind-embed
> [[Periodic notes navigation - meta bind embed]]
> ```
