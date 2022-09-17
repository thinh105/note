---
id: vh9aqyknerxv90wq2v1mebr
title: Vuetify
desc: ''
updated: 1663409164564
created: 1663409164564
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Vuetify
updated_imported: '2020-06-04T08:34:12.000Z'
created_imported: '2020-06-04T08:30:09.000Z'
---

[TOC]












# [Meaning of v-slot:activator=“{ on }”](https://stackoverflow.com/questions/55188478/meaning-of-v-slotactivator-on)


**Ex: Menu in Vuetify**
```js
<template>
  <div class="text-center">
    <v-menu offset-y>
      <template v-slot:activator="{ on }">
        <v-btn
          color="primary"
          dark
          v-on="on"
        >
          Dropdown
        </v-btn>
      </template>
      <v-list>
        <v-list-item
          v-for="(item, index) in items"
          :key="index"
          @click=""
        >
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </div>
</template>
```



The following line declares a [scoped slot](https://vuejs.org/v2/guide/components-slots.html#Scoped-Slots) named `activator`, and it is provided a scope object (from `VMenu`), which contains a property named `on`:

```html
<template v-slot:activator="{ on }">
```

This uses [destructuring syntax](https://vuejs.org/v2/guide/components-slots.html#Destructuring-Slot-Props) on the scope object, which [IE does not support](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Browser_compatibility).

For IE, you'd have to dereference `on` from the scope object itself:

```html
<template v-slot:activator="scope">
  <v-toolbar-title v-on="scope.on">
```




## Details on the `activator` slot

[`VMenu`](https://vuetifyjs.com/en/components/menus#slots) allows users to specify a slotted template named `activator`, containing component(s) that activate/open the menu upon certain events (e.g., `click`). `VMenu` provides listeners for those events [via an object](https://github.com/vuetifyjs/vuetify/blob/1b4dbae58cbfdeda4edbc14104a53ce9c0a36f62/packages/vuetify/src/components/VMenu/mixins/menu-generators.js#L22), passed to the `activator` slot:

```html
<v-menu>
  <template v-slot:activator="scopeDataFromVMenu">
    <!-- slot content goes here -->
  </template>
</v-menu>
```

The slot content can access `VMenu`'s event listeners like this:

```html
<v-menu>
  <template v-slot:activator="scopeDataFromVMenu">
    <button v-on="scopeDataFromVMenu.on">Click</button>
  </template>
</v-menu>
```

For improved readability, the scoped data can also be [destructured](https://vuejs.org/v2/guide/components-slots.html#Destructuring-Slot-Props) in the template:

```html
<!-- equivalent to above -->
<v-menu>
  <template v-slot:activator="{ on }">
    <button v-on="on">Click</button>
  </template>
</v-menu>
```

The listeners from the scope object are passed to the `<button>` with [`v-on`](https://vuejs.org/v2/api/#v-on)'s object syntax, which binds one or more event/listener pairs to the element. For this value of `on`:

```js
{
  click: activatorClickHandler // activatorClickHandler is an internal VMenu mixin
}
```

...the button's click handler is bound to a `VMenu` method.

  
  
  
  

