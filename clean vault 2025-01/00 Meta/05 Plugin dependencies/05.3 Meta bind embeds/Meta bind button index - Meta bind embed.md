
%% # Periodic notes buttons %%


```meta-bind-button
label: ""
icon: calendar-search
hidden: true
class: ""
tooltip: Periodic notes date switcher
id: periodic-notes-date-switcher
style: default
actions:
  - type: command
    command: periodic-notes:show-date-switcher
```

%% ## Daily notes %%
```meta-bind-button
label: Create morning pages section
icon: ""
hidden: true
class: ""
tooltip: Insert Morning Pages section
id: morning-pages
style: default
actions:
  - type: "regexpReplaceInNote"
    regexp: "^# Log"
    regexpFlags: "gm"
    replacement: "# Morning pages\n<%tp.file.cursor(1)%>\n\n# Log"
  - type: sleep
    ms: 100
  - type: command
    command: templater-obsidian:jump-to-next-cursor-location
  - type: command
    command: editor:focus
```

%% ## Weekly notes %%

```meta-bind-button
label: Weekly review checklist
icon: ""
hidden: true
class: ""
tooltip: Insert weekly review checklist template
id: weekly-review
style: default
actions:
  - type: command
    command: templater-obsidian:00 Meta/05 Plugin dependencies/05.1 Templates/Plugin related templates/Periodic notes templates/Periodic notes - checklist - weekly.md
```

%% ## Monthly notes %%

```meta-bind-button
label: Monthly review checklist
icon: ""
hidden: true
class: ""
tooltip: Insert monthly review checklist template
id: monthly-review
style: default
actions:
  - type: "regexpReplaceInNote"
    regexp: "^# Log"
    regexpFlags: "gm"
    replacement: "<%tp.file.include(\"[[Periodic notes - checklist - monthly]]\")%>\n<%tp.file.cursor(1)%>\n\n# Log"
  - type: sleep
    ms: 100
  - type: command
    command: templater-obsidian:replace-in-file-templater
  - type: command
    command: editor:focus
```
%% ## Yearly notes %%

```meta-bind-button
label: Yearly review - last year
icon: ""
hidden: true
class: ""
tooltip: Insert yearly review
id: yearly-review-last-year
style: default
actions:
  - type: command
    command: templater-obsidian:00 Meta/05 Plugin dependencies/05.1 Templates/Periodic notes - checklist - yearly - last year.md
```

```meta-bind-button
label: Yearly review - next year
icon: ""
hidden: true
class: ""
tooltip: Insert yearly review
id: yearly-review-next-year
style: default
actions:
  - type: command
    command: templater-obsidian:00 Meta/05 Plugin dependencies/05.1 Templates/Periodic notes - checklist - yearly - next year.md
```

%% # Banners and Google photos %%

```meta-bind-button
label: ""
icon: image
hidden: true
class: ""
tooltip: Add banner image (Google Photo share link)
id: add-banner-Google-Photo-share-link
style: default
actions:
  - type: command
    command: templater-obsidian:00 Meta/05 Plugin dependencies/05.1 Templates/JS templates/Google photo share link, paste to banner property - JS template.md
```

```meta-bind-button
label: ""
icon: image-down
hidden: true
class: ""
tooltip: Add banner image (direct image URL)
id: add-banner-direct-image-URL
style: default
actions:
  - type: command
    command: templater-obsidian:00 Meta/05 Plugin dependencies/05.1 Templates/JS templates/paste image URL to banner property - JS template.md
```
