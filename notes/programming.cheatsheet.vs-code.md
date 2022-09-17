---
id: ta80spo5glc8hl6qlq1dgni
title: Vs Code
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: VS CODE
updated_imported: '2020-05-25T02:27:12.000Z'
created_imported: '2019-11-07T11:59:00.000Z'
---

[TOC]























# Hot Key

`Ctrl` + `K` + `S`

https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf

## Jump to function/method definition : `CTRL` + `click` on function ~> `Click`/`Alt` + `Click` to the title bar

## Clean one line: `Ctrl` + `Shift` + `K`

## Find quickly file: `Ctrl` + `p` or `Ctrl` + `e`

## Clone a line: `Ctrl` + `Shift` + `Alt` + `↓` (Numpad arrows on Ubuntu)


## Select all the same of selected word : `Ctrl` + `Shift` + `L`

## Select same word one by one manually : `Ctrl` + `D`     

## Soft Undo : Use the Soft Undo (Ctrl+ U)
to move the cursor back to it's previous location.

This is particularly useful when you need to move down in a long file to copy a variable or function name and then go back to your original position.

# ESlint vue

https://medium.com/@agm1984/how-to-set-up-es-lint-for-airbnb-vue-js-and-vs-code-a5ef5ac671e8

# restClient - VSCODE correct form

Follows the rules in RFC, there should be a separated blank line between header and body parts.

```js

//WRONG
POST http://localhost:55200/test HTTP/1.1
content-type: application/json
{
    "cname": "weile"
}

//CORRECT
POST http://localhost:55200/test HTTP/1.1
content-type: application/json

{
    "cname": "weile"
}
```

[@source](https://github.com/Huachao/vscode-restclient/issues/430)



# How do I select an entire line of code as quickly as possible?

All you need to do is put the cursor anywhere on the line, do not make any selection at all and then do the desired command (Cut, copy, or paste).

When there is no text selected, cut, copy and paste default to selecting the entire line. Try it!




# External Resource
https://vscodecandothat.com/
