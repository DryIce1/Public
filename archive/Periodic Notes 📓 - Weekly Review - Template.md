---
banner: "https://www.swellnet.com/sites/default/files/styles/swellnet_large_wm_630x443/public/wave-of-the-day/5d4_6328.jpg?itok=DGgzTjGB"
banner_y: 0.75
---
# <% tp.date.now("dddd Do of MMMM", 0, tp.file.title, "YYYY-[W]ww") %> to <% tp.date.now("dddd Do of MMMM", +6, tp.file.title, "YYYY-[W]ww") %>
> [!note]- Temporal Information 
> - #log/7
> - **Days**:: [[<% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +1, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +2, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +3, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +4, tp.file.title, "YYYY-[W]ww") %>]], [[<% tp.date.now("YYYY-MM-DD", +5, tp.file.title, "YYYY-[W]ww") %>]],  [[<% tp.date.now("YYYY-MM-DD", +6, tp.file.title, "YYYY-[W]ww") %>]]
> - **Last Week**:: [[<%tp.date.now("YYYY-[W]ww", -7, tp.file.title, "YYYY-[W]ww")%>]]  
> - **Month**:: [[<%tp.date.now("YYYY-MM", +6, tp.file.title, "YYYY-[W]ww")%>]]  

> [!check]+ ###### Review Guiding Principles - *quick skim of bullet points*
> ![[Guiding Principles Development#Guiding Principles]]

###### Reflections 
> [!check]+ Review Past Week
> - In <% tp.date.now("MMMM YYYY", 0, tp.file.title, "YYYY-[W]ww") %>, my theme is **`=[[<% tp.date.now("YYYY-MM", 0, tp.file.title, "YYYY-[W]ww") %>]].theme`**, in the context of **`=[[<% tp.date.now("YYYY-MM", 0, tp.file.title, "YYYY-[W]ww") %>]].year.theme`**
> - From last week, I wanted to improve by **`=this.last-week.working-towards`**
> - From last month, I wanted to improve by **`=this.month.last-month.working-towards`**

> [!check]+ Sleep and Energy Tracking
> - *Weekly Sleep Duration Average calculation*: `=round(sum(filter(this.days.sleep-duration, (p) => p)) / length(filter(this.days.sleep-duration, (p) => p)), 2) + " hours"`
> - *Round the decimal*: `=round(0.<%tp.file.cursor(1)%>*60)` minutes
> - *Sleep Comments and quality this week*: `=this.days.sleep-comments`
> ```dataview
> table WITHOUT ID
>	file.link as Date_____, 
>	join(list(sleep-hour, sleep-minute), ":") as "Sleep Time",
>	join(list(awake-hour, awake-minute), ":") as "Awake Time", 
>	sleep-duration as "Sleep Duration",
>	choice(physical-activity, "✅", "❌") as "Physical Activity",
>	sleep-comments as "Sleep Comments"
> FROM #📓
> WHERE week = this.file.link
> SORT file.name asc
- **Sleep Duration**:: 
- **Sleep Comments**:: 

> [!tip]+ *This week I was grateful for*: 
> 
>```dataview
>TABLE WITHOUT ID
> 	dateformat(days.file.day, "ccc dd ") as Date, 
> 	days.gratitude as "Gratitude"
> FROM "My/Journal"
> WHERE file.name = this.file.name
> FLATTEN days
> WHERE days.gratitude
>```
- **Gratitude**:: 

> [!tip]+ *This week I was proud of*: 
> 
>```dataview
>TABLE WITHOUT ID
> 	dateformat(days.file.day, "ccc dd ") as Date, 
> 	days.highlights as "Highlights"
> FROM "My/Journal"
> WHERE file.name = this.file.name
> FLATTEN days
> WHERE days.highlights
>```
- **Highlights**:: 

> [!tip]+ *This week I was disappointed by*: 
> 
>```dataview
>TABLE WITHOUT ID
> 	dateformat(days.file.day, "ccc dd ") as Date, 
> 	days.disappointments as "Disappointments"
> FROM "My/Journal"
> WHERE file.name = this.file.name
> FLATTEN days
> WHERE days.disappointments
>```
- **Disappointments**:: 

> [!tip]+ *This week I wanted to improve*: 
> 
>```dataview
>TABLE WITHOUT ID
> 	dateformat(days.file.day, "ccc dd ") as Date, 
> 	days.working-towards as "Working Towards"
> FROM "My/Journal"
> WHERE file.name = this.file.name
> FLATTEN days
> WHERE days.working-towards
>```
- **Working Towards**:: 

- **Top Notes**:: 
- **Top Sources**:: 
---
> [!Tip]- Journal
> > [!Monday]+ 
> >
> > ![[<% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-[W]ww") %>#Journal]]
> 
> > [!Tuesday]+ 
> >
> > ![[<% tp.date.now("YYYY-MM-DD", +1, tp.file.title, "YYYY-[W]ww") %>#Journal]]
> 
> > [!Wednesday]+ 
> >
> > ![[<% tp.date.now("YYYY-MM-DD", +2, tp.file.title, "YYYY-[W]ww") %>#Journal]]
> 
> > [!Thursday]+ 
> >
> > ![[<% tp.date.now("YYYY-MM-DD", +3, tp.file.title, "YYYY-[W]ww") %>#Journal]]
> 
> > [!Friday]+ 
> >
> > ![[<% tp.date.now("YYYY-MM-DD", +4, tp.file.title, "YYYY-[W]ww") %>#Journal]]
> 
> > [!Saturday]+ 
> >
> > ![[<% tp.date.now("YYYY-MM-DD", +5, tp.file.title, "YYYY-[W]ww") %>#Journal]]
> 
> > [!Sunday]+ 
> >
>  > ![[<% tp.date.now("YYYY-MM-DD", +6, tp.file.title, "YYYY-[W]ww") %>#Journal]]
---
> [!tip]- Tasks this week
>
> ```dataview
> TASK
> FROM "My/Journal"
> 	WHERE file.day >= date(this.days[0]) 
> 	WHERE file.day <= date(this.days[6])
> 	WHERE (meta(section).subpath = "Journal")
> GROUP BY dateformat(file.day, "cccc dd MMMM") as date
> SORT file.day asc
>```

---

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

