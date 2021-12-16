---
banner: https://wallpaperbat.com/down/192551-super-ultra-wide-wallpaper-hdrshooter
banner_x: 0.5
banner_y: 0.405
---

##### <% tp.date.now("dddd Do of MMMM", 0, tp.file.title, "YYYY-[W]ww") %> to <% tp.date.now("dddd Do of MMMM", +6, tp.file.title, "YYYY-[W]ww") %>
##### Review Guiding Principles - *quick skim of bullet points*
![[Guiding Principles Development#Guiding Principles]]

## Review Past Week
- In <% tp.date.now("MMMM YYYY", 0, tp.file.title, "YYYY-[W]ww") %>, my theme is **`=this.month.theme`**, in the context of **`=this.month.year.theme`**
	- From last week, I wanted to improve by **`=this.last-week.to-improve`**
	- From last month, I wanted to improve by **`=this.month.last-month.to-improve`**

##### Sleep and Energy Tracking
- *Weekly Sleep Average calculation*: `=round(sum(this.days.sleep-duration)/7, 4)` 
- *Round the decimal*: `=round(0.<%tp.file.cursor(1)%>*60)` minutes
- **Sleep Duration**:: 
- **Sleep Comments**:: 
```dataview
table WITHOUT ID
	file.link as Date_____, 
	join(list(sleep-hour, sleep-minute), ":") as "Sleep Time",
	join(list(awake-hour, awake-minute), ":") as "Awake Time", 
	sleep-duration as "Sleep Duration",
	choice(physical-activity, "✅", "❌") as "Physical Activity",
	sleep-comments as "Sleep Comments"
FROM #📓
WHERE week = this.file.link
sort file.name asc
```

##### Reflections 
###### Habits, Priority, Focus
- **Gratitude**:: 
	- *This week I was grateful for*: `=this.days.gratitude`
- **Desired Outcome**:: 
	- *This week I wanted to...*: `=this.days.desired-outcome`
- **Highlights**:: 
	- *This week I was proud of*: `=this.days.highlights`
- **Disappointments**:: 
	- *This week I was disappointed by*: `=this.days.disappointments`
- **To Improve**:: 
	- *This week I wanted to improve*: `=this.days.to-improve`
###### Top Notes
- **Top Notes**:: 
	- *This week my journal linked to these Top Notes*: `=this.days.file.sectionmap["Journal"].outlinks`
	- 
		- ![[<% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-[W]ww") %>#Journal]]
	- 
		- ![[<% tp.date.now("YYYY-MM-DD", +1, tp.file.title, "YYYY-[W]ww") %>#Journal]]
	- 
		- ![[<% tp.date.now("YYYY-MM-DD", +2, tp.file.title, "YYYY-[W]ww") %>#Journal]]
	- 
		- ![[<% tp.date.now("YYYY-MM-DD", +3, tp.file.title, "YYYY-[W]ww") %>#Journal]]
	- 
		- ![[<% tp.date.now("YYYY-MM-DD", +4, tp.file.title, "YYYY-[W]ww") %>#Journal]]
	- 
		- ![[<% tp.date.now("YYYY-MM-DD", +5, tp.file.title, "YYYY-[W]ww") %>#Journal]]
	-  
		-  ![[<% tp.date.now("YYYY-MM-DD", +6, tp.file.title, "YYYY-[W]ww") %>#Journal]]


## Organisation 
###### Clean up 
- **Computer based**
- [ ] Email Inbox Organized & Archived
- [ ] Desktop & Download Folders organised 
- [ ] Clean-up any screenshots or other clutter in routine folders
- [ ] Review finances 
- **Paper Processing**
	- [ ]  Process Physical Inbox
	- [ ]  Pay any Paper Bills
- **House keeping** 
	- [ ] Bedroom, clothes, laundry 

###### Calendar Review
- [ ] Review last 2 week's calendar ⇒ **Does anything need follow-up?**
- [ ] Review the coming 3 week's calendar ⇒ **Does anything need preparation?**

###### Projects 
- [ ] Review **"Waiting On" Actions**
- [ ] Review where my projects stand - give them all at least one next action item

#### well done for finishing the week strongly 

%%
##### Temporal Information
- #📓/7
- **Days**:: [[<% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +1, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +2, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +3, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +4, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +5, tp.file.title, "YYYY-[W]ww") %>]],  [[<% tp.date.now("YYYY-MM-DD", +6, tp.file.title, "YYYY-[W]ww") %>]]
- **Last Week**:: [[<%tp.date.now("YYYY-[W]ww", -7, tp.file.title, "YYYY-[W]ww")%>]]  
- **Month**:: [[<%tp.date.now("YYYY-MM", +0, tp.file.title, "YYYY-[W]ww")%>]]  
%%
