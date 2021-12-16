---
banner: https://cdn.wallpapersafari.com/8/5/cYaDFd.jpg
banner_x: 0.5
banner_y: 0.38
aliases: <% tp.date.now("dddd MMMM Do YYYY", 0, tp.file.title, "YYYY-MM-DD") %>
---
###### Journal
<%* if (tp.date.now("ddd") == "Sun") { %>- [ ] Do Weekly Review Note
- **Review Cycles**:: Weekly<%* } %>
<%* if (tp.date.now("D") == 1){ %>- [ ] Do Monthly Review Note  - Set Theme for the month
- **Review Cycles**:: Monthly<%* } %>
<%* if (tp.date.now("MM-DD") == "01-01"){ %>- [ ] Do Yearly Note  
- **Review Cycles**:: Yearly<%* } %>

---
## 🌅 Morning
- Themes
	- In *<% tp.date.now("MMMM YYYY", 0, tp.file.title, "YYYY-MM-DD") %>*, my theme is **`=this.week.month.theme`**, in the context of **`=this.week.month.year.theme`**
-  Improvements
	- *Yesterday* I wanted to **`=this.yesterday.to-improve`**
	- *Last week* I wanted to **`=this.week.last-week.to-improve`**
- How did you **sleep**? 
	- **Sleep Hour**:: 
	- **Sleep Minute**:: 0
	- **Awake Hour**:: 
	- **Awake Minute**:: 0
	- *Calculate duration*: `=round(sum(24 - sum(this.sleep-hour + (this.sleep-minute / 60)) + sum(this.awake-hour + (this.awake-minute / 60))), 4)` hours
	- **Sleep Duration**:: 
	- **Sleep Comments**:: 
- Start the day and reflections
	- **Gratitude**:: 
	- **Desired Outcome**:: 
	- Flashcards:: 

---
- Work Start Time:: 
- Work Finish Time:: 
- Work Comments:: 
---
## 🌒 Evening Wind Down
- *Today I wanted to*: **`=this.desired-outcome`**
- **Highlights**:: 
- **Disappointments**:: 
- **Gratitude**:: 
- **To Improve**:: 

%%
###### Hidden Temporal Information
- #📓/☀️
- Yesterday:: [[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>]]
- **Week**:: [[<%tp.date.now("YYYY-[W]ww", +0, tp.file.title, "YYYY-MM-DD")%>]]  
%%
