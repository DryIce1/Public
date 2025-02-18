---
up:
  - "[[03 Datascopes (folder note)]]"
related:
  - "[[Meta bind - datascope]]"
aliases: 
tags: 
banner: 
publish: true
date-created: 2024-11-05
cssclasses:
  - dvl-c
  - wide-dataview
searchBind: 
typeBind: 
statusBind: 
priorityBind: 
ratingBind: 
authorBind: 
limitBind: 
date-createdBind: 
dateRangeYearsBind: 
dateRangeDaysBind: 
urgentBind: false
importantBind: false
sortBind: 
tagBind: 
folderBind: 
groupBind: 
coverBind: false
---


> [!multi-column]
> 
>> [!search|cmin]+ filter
>>  `BUTTON[resetProperties]`  
>>
>>> [!property]+ filter by **search** term 
>>> ```meta-bind
>>> INPUT[list(placeholder('search')
>>> ):searchBind]
>>> ```
>>>`BUTTON[resetSearchBind]`
>>
>>> [!property]+ Filter by **author**
>>> ```meta-bind
>>> INPUT[list(placeholder('author')
>>> ):authorBind]
>>> ```
>>> `BUTTON[resetAuthorBind]`
>>
>
>
>> [!search|cmin]+ filter
>>
>>> [!property]- select **urgent & important**
>>> |                                                                |                                                                      |
>>> | -------------------------------------------------------------- | -------------------------------------------------------------------- |
>>> | **urgent**                                                     | **important**                                                        |
>>> | `INPUT[toggle():urgentBind]` | `INPUT[toggle():importantBind]` |
>>
>>> [!property]- toggle for **covers or banners** 
`INPUT[toggle():coverBind]`
>>
>>> [!property]- select **rating** 
>>> `INPUT[number():ratingBind]` (`VIEW[{ratingBind}][]`) `BUTTON[resetRatingBind]`
>>> ```meta-bind
>>> INPUT[slider(addLabels, minValue(0), maxValue(7), stepSize(1)):ratingBind]
>>> ```
>>
>>> [!property]- filter by **date created**
>>> - `INPUT[datePicker():date-createdBind]`
>>>    - `BUTTON[resetDate]`
>>> - Date range :`INPUT[number():dateRangeYearsBind]` years
>>> - Date range :`INPUT[number():dateRangeDaysBind]` days
>>>> [!date]- filter by date created logic
>>>> - if `date-createdBind` is `null`/unset, all notes are included in the search.
>>>> - if `date-createdBind` is set, the default filter range is +/- 90 days.
>>>> - the date range binding allows one to choose the number of days and/or years to include in the search, in both directions.
>
>> [!search] select display
>> 
>> - Query results **limit** `INPUT[number(placeholder(limit default: 150)):limitBind]`
>> 
>> ## select CSS classes
>> ```meta-bind
>> INPUT[multiSelect(option(wide-dataview), option(dvl-c)):cssclasses]
>> ```
>>
>>`BUTTON[edit-meta-bind-embed]`

> [!multi-column]
>
>> [!search-folder]+ Select type
>>
>> ```meta-bind
> > INPUT[multiSelect(
> > option(note), 
> > option(medical_note), 
> > option(my), 
> > option(person), 
> > option(recipe),
> > option(book), 
> > option(article), 
> > option(author), 
> > option(film),
> > option(series), 
> > option(podcast_channel), 
> > option(podcast_episode),
> > option(LLM)
> > ):typeBind]
> > ```
>>  `BUTTON[resetTypeBind]`
>
>> [!search]+ Select status
>>
>> ```meta-bind
>> INPUT[multiSelect(
>> option(not started), 
>> option(scheduled),
>> option(active), 
>> option(paused), 
>> option(waiting), 
>> option(done), 
>> option(cancelled)
>> ):statusBind]
>> ```
>
>> [!search-folder]+ Filter by **folder**
>>```meta-bind
>> INPUT[multiSelect(
>> option(00 Meta),
>> option(10 Sources), 
>> option(20 My),
>> option(20 My/21 Journal), 
>> option(20 My/22 Reflections),
>> option(20 My/24 People),
>> option(30 Notes), 
>> option(30 Notes/31 Medicine), 
>> option(40 Projects), 
>> option('', root folder)
>> ):folderBind]
>> ```

`BUTTON[resetProperties]`  

```meta-bind-button
label: ""
icon: file-cog
hidden: true
class: ""
tooltip: edit meta-bind embed
id: edit-meta-bind-embed
style: default
actions:
  - type: open
    link: "[[Meta bind datascope - embed]]"
    newTab: true
```


```meta-bind-button
label: "Reset All Filters"
style: destructive
id: "resetProperties"
hidden: true
actions:
- type: updateMetadata
  bindTarget: searchBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: typeBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: statusBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: priorityBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: ratingBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: authorBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: folderBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: limitBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: date-createdBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: dateRangeYearsBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: dateRangeDaysBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: urgentBind
  evaluate: false
  value: false
- type: updateMetadata
  bindTarget: importantBind
  evaluate: false
  value: false
- type: updateMetadata
  bindTarget: coverBind
  evaluate: false
  value: false
- type: updateMetadata
  bindTarget: sortBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: groupBind
  evaluate: false
  value: null
```

```meta-bind-button
label: "reset date"
style: primary
id: "resetDate"
hidden: true
actions:
- type: updateMetadata
  bindTarget: date-createdBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: dateRangeDaysBind
  evaluate: false
  value: null
- type: updateMetadata
  bindTarget: dateRangeYearsBind
  evaluate: false
  value: null
```

```meta-bind-button
label: "reset search"
style: primary
id: "resetSearchBind"
hidden: true
actions:
- type: updateMetadata
  bindTarget: searchBind
  evaluate: false
  value: null
```

```meta-bind-button
label: "reset author"
style: primary
id: "resetAuthorBind"
hidden: true
actions:
- type: updateMetadata
  bindTarget: authorBind
  evaluate: false
  value: null
```

```meta-bind-button
label: "reset type"
style: primary
id: "resetTypeBind"
hidden: true
actions:
- type: updateMetadata
  bindTarget: typeBind
  evaluate: false
  value: null
```

```meta-bind-button
label: "reset rating"
style: primary
id: "resetRatingBind"
hidden: true
actions:
- type: updateMetadata
  bindTarget: ratingBind
  evaluate: false
  value: null
```

