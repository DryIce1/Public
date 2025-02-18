<%"---"%>
yesterday: 
  - "[[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>]]"
week: 
  - "[[<%tp.date.weekday("YYYY-[W]ww", +0, tp.file.title, "YYYY-MM-DD")%>]]" 
date-created: <% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-MM-DD") %>
aliases: <% tp.date.now("dddd MMMM Do YYYY", 0, tp.file.title, "YYYY-MM-DD") %>
tags: log/day
cssClasses:
  - log
publish: false
banner: 
banner-y: 
cover: 
url: https://photos.google.com/u/0/search/<% tp.date.now("MMMM[%20]YYYY", 0, tp.file.title, "YYYY-MM") %>
<%* if (tp.file.title === tp.date.now()) { %>sleep: <% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD")%>T<% await tp.system.suggester(["Sleep hour", "20", "21", "22", "23"], ["00", "20", "21", "22", "23"]) %>:<% await tp.system.suggester(["Sleep minute", "00", "05", "10", "15", "20", "25", "30", "35", "40", "45", "50", "55"], ["01", "00", "05", "10", "15", "20", "25", "30", "35", "40", "45", "50", "55"]) %><%* } else { %>sleep: <%* } %>
<%* if (tp.file.title === tp.date.now()) { %>awake: <% tp.date.now("YYYY-MM-DD", 0, tp.file.title, "YYYY-MM-DD")%>T<% await tp.system.suggester(["Awake hour", "05", "06", "07", "08", "09"], [ "00", "05", "06", "07", "08", "09"]) %>:<% await tp.system.suggester(["Awake minute", "00", "05", "10", "15", "20", "25", "30", "35", "40", "45", "50", "55"], ["01", "00", "05", "10", "15", "20", "25", "30", "35", "40", "45", "50", "55"]) %><%* } else { %>awake: <%* } %>
---

```meta-bind-embed
[[Meta bind daily note embed]]
```

# Log<%tp.file.cursor(1)%><%-tp.file.include("[[Periodic Notes Generator (PNG) - template]]")%>
