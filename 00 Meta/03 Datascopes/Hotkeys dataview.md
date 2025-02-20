---
aliases: 
date-created: 2023-02-07
publish: true
tags:
  - atlas/datascope
---

```dataviewjs
const getNestedObject = (nestedObj, pathArr) => {
    return pathArr.reduce((obj, key) =>
        (obj && obj[key] !== 'undefined') ? obj[key] : undefined, nestedObj);
}

function hilite(keys, how) {
    // need to check if existing key combo is overridden by undefining it
    if (keys && keys[1][0] !== undefined) {
        return how + keys.flat(2).join('+').replace('Mod', 'Ctrl') + how;
    } else {
        return how + '–' + how;
    }
}

function getHotkey(arr, highlight=true) {
    let hi = highlight ? '**' : '';
    let defkeys = arr.hotkeys ? [[getNestedObject(arr.hotkeys, [0, 'modifiers'])],
    [getNestedObject(arr.hotkeys, [0, 'key'])]] : undefined;
    let ck = app.hotkeyManager.customKeys[arr.id];
    var hotkeys = ck ? [[getNestedObject(ck, [0, 'modifiers'])], [getNestedObject(ck, [0, 'key'])]] : undefined;
    return hotkeys ? hilite(hotkeys, hi) : hilite(defkeys, '');
}

let cmds = dv.array(Object.entries(app.commands.commands))
    .where(v => getHotkey(v[1]) != '–')
    .sort(v => v[1].id, 'desc')
    .sort(v => getHotkey(v[1], false), 'desc');

dv.paragraph(cmds.length + " commands with assigned hotkeys; " +
    "non-default hotkeys <strong>bolded</strong>.<br><br>");

dv.table(["Name in current locale", "Command ID", "Hotkeys"],
  cmds.map(v => [
    v[1].name,
    v[1].id,
    getHotkey(v[1]),
    ])
  );
```
