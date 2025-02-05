
> [!project]+ Project metadata
> - **Quarter**:: [[<%tp.date.now("YYYY-[Q]Q")%>]]
> - **Priority**:: <% await tp.system.suggester(["P1", "P2", "P3", "P4", "P5"], ["P1", "P2", "P3", "P4", "P5"]) %>
> - **Outcome Goals**:: <% await tp.system.prompt("What are the outcome goals?", "SMART goal") %>
> - **Value Goals**:: <%*



