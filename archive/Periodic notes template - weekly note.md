<%"---"%>
last-week: 
  - "[[<%tp.date.now("YYYY-[W]WW", -7, tp.file.title, "YYYY-[W]WW")%>]]"
month: 
  - "[[<%tp.date.now("YYYY-MM", +6, tp.file.title, "YYYY-[W]WW")%>]]"
days: 
  - "[[<% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-[W]WW") %>]]"
  - "[[<% tp.date.now("YYYY-MM-DD", +1, tp.file.title, "YYYY-[W]WW") %>]]"
  - "[[<% tp.date.now("YYYY-MM-DD", +2, tp.file.title, "YYYY-[W]WW") %>]]"
  - "[[<% tp.date.now("YYYY-MM-DD", +3, tp.file.title, "YYYY-[W]WW") %>]]"
  - "[[<% tp.date.now("YYYY-MM-DD", +4, tp.file.title, "YYYY-[W]WW") %>]]"
  - "[[<% tp.date.now("YYYY-MM-DD", +5, tp.file.title, "YYYY-[W]WW") %>]]"
  - "[[<% tp.date.now("YYYY-MM-DD", +6, tp.file.title, "YYYY-[W]WW") %>]]"
date-created: <% tp.date.now("YYYY-MM-DD", +0, tp.file.title, "YYYY-[W]WW") %>
aliases: 
tags: log/week
cssClasses:
  - log
publish: false
banner: 
banner-y: 
cover: 
<%"---"%>

```meta-bind-embed
[[Meta bind weekly note embed]]
```

# Log<%tp.file.cursor(0)%>