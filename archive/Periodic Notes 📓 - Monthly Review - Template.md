---
banner: "https://wallpapercave.com/wp/wp4200965.jpg"
banner_y: 0.445
aliases: <% tp.date.now("MMMM YYYY", +0, tp.file.title, "YYYY-MM") %>
---
# <% tp.date.now("MMMM YYYY", +0, tp.file.title, "YYYY-MM") %>
> [!NOTE]- Temporal Information
> - #log/📆 
> > Do I need to **add a 5th week**?
> - **Weeks**:: [[<% tp.date.now("YYYY-[W]ww", +0, tp.file.title, "YYYY-MM") %>]], [[<% tp.date.now("YYYY-[W]ww", +7, tp.file.title, "YYYY-MM") %>]], [[<% tp.date.now("YYYY-[W]ww", +14, tp.file.title, "YYYY-MM") %>]], [[<% tp.date.now("YYYY-[W]ww", +21, tp.file.title, "YYYY-MM") %>]]
> - **Last Month**:: [[<% tp.date.now("YYYY-MM", -15, tp.file.title, "YYYY-MM") %>]]
> - **Quarter**:: [[<% tp.date.now("YYYY-[Q]Q", +0, tp.file.title, "YYYY-MM") %>]]
> - **Year**:: [[<% tp.date.now("YYYY", +0, tp.file.title, "YYYY-MM") %>]]

> [!check]+  Review past month
> Yearly Theme <% tp.date.now("YYYY", +0, tp.file.title, "YYYY-MM") %>: *`=this.year.theme`* 
> - *Last Month's theme*: `=this.last-month.theme`
- **Theme**:: <%tp.file.cursor(0)%>

> [!check]+ Sleep and Energy Tracking
> - *Monthly Sleep Duration Average calculation*: `=round(sum(filter(this.weeks.sleep-duration, (p) => p)) / length(filter(this.weeks.sleep-duration, (p) => p)), 2)`
> - *Round the decimal*: `=round(0.<%tp.file.cursor(1)%>*60)` minutes
> - *Sleep quality this month*: `=this.weeks.sleep-comments`
> ```dataview
> table WITHOUT ID
> 	file.link as Date____, 
> 	sleep-duration as "Sleep Duration",
> 	sleep-comments as "Sleep Comments",
> 	choice(physical-activity, "✅", "❌") as "Physical Activity"
> FROM #📓/7 
> WHERE month = this.file.link
> sort file.name asc
> ```
- **Sleep Duration**:: 
- **Sleep Comments**:: 

##### Reflections
> [!tip]+ *This Month my "Gratitude Reflections" were*: 
> 
> `=this.weeks.gratitude`
- **Gratitude**:: 
> [!tip]+ *This Month my "Highlights" were*: 
> 
> `=this.weeks.highlights`
- **Highlights**:: 
> [!tip]+ *This Month my "Disappointments" were*: 
> 
> `=this.weeks.disappointments`
- **Disappointments**:: 
> [!tip]+ *This Month my "Working Towards" and "improvements" reflections were*: 
> 
> `=this.weeks.working-towards`
- **Working Towards**:: 
> [!tip]+ *This month, my "Top Notes" were:* 
> 
> `=this.weeks.top-notes`
- **Top Notes**:: 
> [!tip]+ *This month, my "Top Sources" were:* 
> 
> `=this.weeks.top-sources`
- **Top Sources**:: 

# Alignment review
- [ ] Review [[Pillars 🔺 - Foundational Areas]] 
- [ ] Review [[Habits & Routines 🧬]] 
- [ ] Review fitness plan - **View Workout Tracking Database**
- [ ] Review & Refine [[Mindset MOC]] page
- [ ] View Daily Tracking

# Projects and Goals Review
- [ ] Prioritize Projects - determine "Active" Projects for the upcoming month
- [ ] Review Outcome Goals
- [ ] Mark any "Completed" Goals
- [ ] Update Goal Timelines (Dates & Quarters)

# Vaults - files & spaces
- [ ] Empty Recycling Bin on Computer
- [ ] Tidy up [[Interests]]
- [ ] Review media in [[Library - Books & Reading Lists]] and [[Media Library - Master Collection]]

#### well done for finishing <% tp.date.now("MMMM", +0, tp.file.title, "YYYY-MM") %> strongly!

