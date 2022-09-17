---
id: h9ss8pxzkz369hgk8ybls02
title: Performance
desc: ''
updated: 1663409164563
created: 1663409164563
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Performance
updated_imported: '2022-01-16T04:25:46.000Z'
created_imported: '2022-01-08T04:04:51.000Z'
---

[TOC]













* * *


# [The Power Of Not Mutating Data](https://reactjs.org/docs/optimizing-performance.html#the-power-of-not-mutating-data)

The simplest way to avoid this problem is to avoid mutating values that you are using as props or state. For example, the handleClick method above could be rewritten using concat as:

```js
handleClick() {
  this.setState(state => ({
    words: state.words.concat(['marklar'])
  }));
}
```

ES6 supports a spread syntax for arrays which can make this easier. If you’re using Create React App, this syntax is available by default.

```js
handleClick() {
  this.setState(state => ({
    words: [...state.words, 'marklar'],
  }));
};
```
You can also rewrite code that mutates objects to avoid mutation, in a similar way. For example, let’s say we have an object named colormap and we want to write a function that changes colormap.right to be 'blue'. We could write:
```js
function updateColorMap(colormap) {
  colormap.right = 'blue';
}
```
To write this without mutating the original object, we can use Object.assign method:
```js
function updateColorMap(colormap) {
  return Object.assign({}, colormap, {right: 'blue'});
}
```
`updateColorMap` now returns a new object, rather than mutating the old one. `Object.assign` is in ES6 and requires a polyfill.

Object spread syntax makes it easier to update objects without mutation as well:
```js
function updateColorMap(colormap) {
  return {...colormap, right: 'blue'};
}
```
This feature was added to JavaScript in ES2018.

If you’re using Create React App, both `Object.assign` and the object spread syntax are available by default.

# Dealing with Deeply Nested Objects

When you deal with deeply nested objects, updating them in an immutable way can feel convoluted.

If you run into this problem, check out [Immer](https://github.com/mweststrate/immer) or [immutability-helper](https://github.com/kolodny/immutability-helper).

These libraries let you write highly readable code without losing the benefits of immutability.

@source

# Immutability in React (and also JS)

Read further:

https://stackoverflow.com/questions/34385243/why-is-immutability-so-important-or-needed-in-javascript