<%"---"%>
last-year:
  - "[[<% tp.date.now("YYYY", -1, tp.file.title, "YYYY") %>]]"
decade:
  - "[[<% tp.file.title.slice(0, 3) + '0-' + tp.file.title.slice(0, 3) + '9' %>]]"
5-year: 
  - "[[<%* const year = parseInt(tp.file.title); const start = Math.floor(year / 5) * 5; const end = start + 4; tR += start + "-" + end; %>]]"
quarters:
  - "[[<% tp.date.now("YYYY-[Q]1", +0, tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-[Q]2", +0, tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-[Q]3", +0, tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-[Q]4", +0, tp.file.title, "YYYY") %>]]"
months:
  - "[[<% tp.date.now("YYYY-MM", "P0M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P1M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P2M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P3M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P4M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P5M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P6M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P7M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P8M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P9M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P10M", tp.file.title, "YYYY") %>]]"
  - "[[<% tp.date.now("YYYY-MM", "P11M", tp.file.title, "YYYY") %>]]"
date-created: <% tp.date.now("YYYY-01-01", +0, tp.file.title, "YYYY") %>
aliases: 
tags: log/year
type: log/year
cssClasses:
  - log
publish: false
banner: 
banner-y: 
cover: 
<%"---"%>


```meta-bind-embed
[[Periodic notes yearly note - meta bind embed]]
```
