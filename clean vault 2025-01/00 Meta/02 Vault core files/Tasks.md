---
up:
  - "[[Home]]"
related:
  - https://obsidian-tasks-group.github.io/obsidian-tasks/queries/
aliases:
  - To do
date-created: 2023-01-01
banner: https://i.imgur.com/1gMSkSr.png
banner-y: 50
cssclasses: 
publish: false
tags:
  - atlas/core
---

> [!Attention] Priority 
> ```tasks
> (status.type is TODO) OR (status.name includes In progress)
> (done today) OR (not done) AND NOT ((done date is invalid) OR (due date is invalid) OR (scheduled date is invalid) OR (start date is invalid)) 
> tag does not include medicine
> happens before tomorrow
> show urgency
> sort by status.type
> limit 1
> short
> ```

```tasks
    (status.type is TODO) OR (status.name includes In_progress)
(done today) OR (not done) AND NOT ((done date is invalid) OR (due date is invalid) OR (scheduled date is invalid) OR (start date is invalid))
tag does not include chore
tag does not include medicine
happens before tomorrow
show urgency
sort by status.type
limit 50
short
```

> [!NOTE]- Group by tag
> %% tag regex does not match /#.*/i  %%
> <sup>*If a task has another another tag (e.g. `#status/active`)* it will be shown here. But this list does not show recurring tasks.</sup>
> ```tasks 
> tag regex matches /#.*/i 
> (done today) OR (not done) 
> is not recurring
> group by tags
> sort by status.type
> show urgency
> ```

> [!tldr]+ Inbox
> <sup>Aim for **inbox zero**!</sup>
> <sup>*These `#task`s have no other tags and no "happens" date*</sup>
> ```tasks
> tag regex does not match /#.*/i 
> (not done) AND (no happens date) OR (not done) AND ((done date is invalid) OR (due date is invalid) OR (scheduled date is invalid) OR (start date is invalid))
> show urgency
> ```

> [!example]- Upcoming tasks this month
> <sup>*Happens within 6 weeks*</sup>
> ```tasks
> (not done) AND NOT ((done date is invalid) OR (due date is invalid) OR (scheduled date is invalid) OR (start date is invalid))
> (happens after today) AND (happens before in 6 weeks)
> group by happens 
> show urgency
> ```
¢
> [!help]- Task status is *Waiting for* or *In progress* or *Someday*
> ```tasks 
> (status.type is IN_PROGRESS) OR (status.name includes someday)
> (done today) OR (not done) OR (status.name includes Someday)
> group by status.name
> sort by status.type
> short
> ```

> [!NOTE]- Recurring tasks
> ```tasks 
> not done
> is recurring
> sort by priority
> group by happens
> short
> ```

> [!check]- Done
> ```tasks 
> done after last week
> group by done
> short
> explain
> ```

> [!NOTE]- [[All Tasks]] log

