<%"---"%>
yesterday: 
  - "[[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>]]"
week: 
  - "[[<%tp.date.weekday("YYYY-[W]WW", +3, tp.file.title, "YYYY-MM-DD")%>]]" 
date-created: <% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-MM-DD") %>
aliases: <% tp.date.now("dddd MMMM Do YYYY", 0, tp.file.title, "YYYY-MM-DD") %>
tags: log/day
type: log/day
cssClasses:
  - log
publish: false
banner: 
banner-y: 
cover: 
url: https://photos.google.com/u/0/search/<% tp.date.now("MMMM[%20]YYYY", 0, tp.file.title, "YYYY-MM") %>
sleep: 
awake:
---

```meta-bind-embed
[[Periodic notes daily note - meta bind embed]]
```

# Log<%tp.file.cursor(1)%><%-tp.file.include("[[Periodic Notes Generator (PNG) - template]]")%>