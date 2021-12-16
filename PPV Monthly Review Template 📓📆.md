---
banner: https://wallpapercave.com/wp/wp4200965.jpg
banner_x: 0.5
banner_y: 0.445
aliases: <% tp.date.now("MMMM YYYY", +0, tp.file.title, "YYYY-MM") %>
---
##### Temporal Information
- #📓/📆 
- **Weeks**:: [[<% tp.date.now("YYYY-[W]ww", +0, tp.file.title, "YYYY-MM") %>]], [[<% tp.date.now("YYYY-[W]ww", +7, tp.file.title, "YYYY-MM") %>]], [[<% tp.date.now("YYYY-[W]ww", +14, tp.file.title, "YYYY-MM") %>]], [[<% tp.date.now("YYYY-[W]ww", +21, tp.file.title, "YYYY-MM") %>]]
- **Last Month**:: [[<% tp.date.now("YYYY-MM", -15, tp.file.title, "YYYY-MM") %>]]
- **Quarter**:: [[<% tp.date.now("YYYY-[Q]Q", +0, tp.file.title, "YYYY-MM") %>]]
- **Year**:: [[<% tp.date.now("YYYY", +0, tp.file.title, "YYYY-MM") %>]]

# <% tp.date.now("MMMM YYYY", +0, tp.file.title, "YYYY-MM") %>
###### Yearly Theme [[<% tp.date.now("YYYY", +0, tp.file.title, "YYYY-MM") %>]]: *`=this.year.theme`* 
- **Theme**:: <%tp.file.cursor(0)%>
	- *Last Month's Focus*: `=this.last-month.theme`
##### Sleep and Energy Tracking
- *Monthly Sleep Average calculation*: `=round(sum(this.weeks.sleep-duration)/length(this.weeks), 2)` 
- *Round the decimal*: `=round(0.<%tp.file.cursor(1)%>*60)` minutes
- **Sleep Duration**:: 
```dataview
table WITHOUT ID
	file.link as Date____, 
	sleep-duration as "Sleep Duration",
	choice(physical-activity, "✅", "❌") as "Physical Activity"
FROM #📓/7 
WHERE month = this.file.link
sort file.name asc
```

##### Reflections
###### Habits, Priority, Focus
- **Gratitude**:: 
	- *This Month my "Gratitude Reflections" were*: `=this.weeks.gratitude`
- **Desired Outcome**:: 
	- *This Month my "Desired Outcomes" were*: `=this.weeks.desired-outcome`
- **Highlights**:: 
	- *This Month my "Highlights" were*: `=this.weeks.highlights`
- **Disappointments**:: 
	- *This Month my "Disappointments" were*: `=this.weeks.disappointments`
- **To Improve**:: 
	- *This Month my "To Improve  Reflections" were*: `=this.weeks.to-improve`

###### Top Notes
- **Top Notes**:: 
	- %%Top notes%%
		```dataview
		table  WITHOUT ID 
			dateformat(file.day, "DDDD") as "Date____", 
			top-notes as "Top Notes and Currently  Working On"
		from #📓/7 
		where top-notes != null
		WHERE month = this.file.link
		SORT file.day desc
		```

# Alignment review
- [ ] Review [[Pillars 🔺 - Foundational Areas]] 
- [ ] Review [[Habits & Routines 🧬]] 
- [ ] Review fitness plan - **View Workout Tracking Database**
- [ ] Review & Refine [[Mindset MOC]] page
- [ ] View Daily Tracking

# Projects and Goals Review
### PROJECTS: **Review & Update**
- [ ] Prioritize Projects - determine "Active" Projects for the upcoming month
### GOALS: **Review & Update**
- [ ] Review Outcome Goals
- [ ] Mark any "Completed" Goals
- [ ] Update Goal Timelines (Dates & Quarters)

# Vaults - files & spaces
- [ ] Empty Recycling Bin on Computer
- [ ] Tidy up [[Interests MOC]]
- [ ] Review media in [[Library of Books & Reading Lists]] and [[Media Library - Master Collection]]

#### well done for finishing the Month strongly!

