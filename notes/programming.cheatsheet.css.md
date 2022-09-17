---
id: dwreewujc5j0cebbnbls6dv
title: CSS
desc: ''
updated: 1663409164559
created: 1663409164559
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: CSS
updated_imported: '2021-01-30T17:32:32.000Z'
created_imported: '2020-07-23T09:17:16.000Z'
---

[TOC]











------------

# set border for easy debug

Show you exactly the element you are working on and what need to do.
```css
 border: 3px solid green;
```

![bad8afe1d5c011dcbedc44ee29e29e71.png](/assets/bad8afe1d5c011dcbedc44ee29e29e71-e9ludxk4pg9d.png)

# [How to center a “position: absolute” element](https://stackoverflow.com/questions/8508275/how-to-center-a-position-absolute-element)

Without knowing the width/height of the positioned1 element, it is still possible to align it as follows:

EXAMPLE HERE
```css
.child {
    position: absolute;
    top: 50%;  /* position the top  edge of the element at the middle of the parent */
    left: 50%; /* position the left edge of the element at the middle of the parent */

    transform: translate(-50%, -50%); /* This is a shorthand of
                                         translateX(-50%) and translateY(-50%) */
}
```
It's worth noting that CSS Transform is supported in IE9 and above. (Vendor prefixes omitted for brevity)

Explanation
Adding top/left of 50% moves the top/left margin edge of the element to the middle of the parent, and translate() function with the (negative) value of -50% moves the element by the half of its size. Hence the element will be positioned at the middle.

This is because a percentage value on top/left properties is relative to the height/width of the parent element (which is creating a containing block).

While a percentage value on translate() transform function is relative to width/height of the element itself (Actually it refers to the size of bounding box).

For unidirectional alignment, go with translateX(-50%) or translateY(-50%) instead.

1. An element with a position other than static. I.e. relative, absolute, fixed values.