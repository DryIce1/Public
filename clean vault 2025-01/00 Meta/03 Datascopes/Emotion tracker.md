---
aliases: 
banner: https://wallpapercave.com/wp/wp10166538.jpg
cssclasses:
  - wide-dataview
date-created: 2023-03-01
description: log of my moods
publish: false
type:
  - datascope
---

> [!NOTE]- By date (without context)
> ```dataview
> TABLE WITHOUT ID 
>     link(file.link, dateformat(time, "HH':'mm EEE, d-MMM")) as time,
>     emotion-group as "Group",
>     "E" + moodMeterEnergy + "P" + moodMeterPleasure as "Mood meter",
>     emotion as "Emotion"
> FROM !"00 Meta"
> WHERE contains(type, "my/emotion-tracker")
> AND default(date-created, file.cday) > date(today) - dur(1 month)
> SORT time desc
> ```

# By date
```dataview
TABLE WITHOUT ID 
    link(file.link, dateformat(time, "HH':'mm EEE, d-MMM")) as time,
    emotion-group as "group",
    "E" + moodMeterEnergy + "P" + moodMeterPleasure as "Mood meter",
    emotion as "emotion",
    activity + choice(who-with, " with " + who-with, ", just myself") + " (" + location + ")" AS context,
    comments
FROM !"00 Meta"
    WHERE contains(type, "my/emotion-tracker")
    WHERE default(date-created, file.cday) > date(today) - dur(1 month)
SORT time desc
```

# Group by emotion group
```dataview
TABLE
    link(rows.file.link, dateformat(rows.time, "HH':'mm EEE, d-MMM")) as time,
    rows.emotion as Emotion,
    rows.location as location
FROM !"00 Meta"
    WHERE contains(type, "my/emotion-tracker")
AND default(date-created, file.cday) > date(today) - dur(3 month)
Group by emotion-group as "Emotion-group"
SORT 
    choice(Emotion-group = "Happiness", "0", 
    choice(Emotion-group = "Love", "1", 
    choice(Emotion-group = "Surprise", "2", 
    choice(Emotion-group = "Anger", "3", 
    choice(Emotion-group = "Sadness", "4", 
    choice(Emotion-group = "Fear", "5", 
    choice(Emotion-group = "Disgust", "6", 
    "7"))))))) asc

```
