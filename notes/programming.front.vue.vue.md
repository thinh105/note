---
id: l4uhrklswsgqfxtajiqgj66
title: Vue
desc: ''
updated: 1663409164564
created: 1663409164564
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Vue
updated_imported: '2020-09-10T01:17:14.000Z'
created_imported: '2020-04-22T03:35:10.000Z'
---

[TOC]













-------------------

# `this` on Vue

## Don't use arrow function in `methods` 

We will lost `this` in that arrow function

```js
// Dont
methods: {
	reloadGrid: () => {
		// code
	}
}

// Do : short-hand method

methods: {
	reloadGrid() {
		// code
	}
}

```

![7b47dc08793187f48c3739e2d4c08386.png](/assets/7b47dc08793187f48c3739e2d4c08386-9s7gkg4q6o2t.png)































# `this` in arrow function

While in ES5 `this` referred to the parent of the function

In ES6, arrow functions use lexical scoping — `this` refers to it’s current surrounding scope and no further. Thus the inner function knew to bind to the inner function only, and not to the object’s method or the object itself.



![68ac5962e589c9bb9e6fb7aadde2c5bf.png](/assets/68ac5962e589c9bb9e6fb7aadde2c5bf-icogpo0coq0s.png)





# nextTick() in Vue

NextTick is a comfortable way to execute a function after the data has been set, and the DOM has been updated.

You need to wait for the DOM, maybe because you need to perform some transformation or you need to wait for an external library to load its stuff? Then use nextTick.

In practice: 


```js
methods: {
      reloadGrid() {
        this.$nextTick(() => {
          this.$refs.stackRef.reflow();
        });
      },
    },
```

# Slot

https://www.smashingmagazine.com/2019/07/using-slots-vue-js/#top

https://vuejsdevelopers.com/2018/02/26/vue-js-reusable-transitions/

# Reset data in component

## vm.$data

The current data object

The data object that the Vue instance is observing. The Vue instance proxies access to the properties on its data object.

## vm.$options.data

`vm.$options` is the initiation options used for the current Vue instance.
This is useful when you want to include custom properties in the options:

```js
new Vue({
  customOption: 'foo',
  created: function () {
    console.log(this.$options.customOption) // => 'foo'
  }
})
```

## snippet code

Copy the initiation data object into current data object.

Using Object assign because if using the spread syntax `...` will get the Vue Warn not allow to change `this.$data`

```js
// no bind the context into data()
Object.assign(this.$data, this.$options.data())
```

Caution, `Object.assign(this.$data, this.$options.data())` does not bind the context into data(). So if you are using this into your data method you may want to apply the context with

```js
// if you are using this into your data method
// you may want to apply the context with call( ) or bind ()
Object.assign(this.$data, this.$options.data.apply(this))

//or

Object.assign(this.$data, this.$options.data.call(this))

```


**Example: learn-vuetify**

```js
export default {
    data: () => ({
      title: '',
      content: '',
      dialog: false,
      menuPickDate: false,
      date: new Date().toISOString().substr(0, 10),
    }),
    computed: {
      computedDateFormattedDatefns() {
        return this.date ? format(parseISO(this.date), 'do MMM yyyy') : '';
      },
    },
    methods: {
      submit() {
        console.log(
          this.$options.data
          //data() {
		  //  return {
		  //    title: 'beng',
		  //    content: 'zing',
		  //    dialog: false,
		  //    menuPickDate: false,
		  //    date: new Date().toISOString().substr(0, 10)
		  //  };
		  // }
        );
        this.dialog = false;
        this.resetData();
      },
      resetData() {
        Object.assign(this.$data, this.$options.data.());
        // this.$data = { ...initialState() };
      },
    },
  };
```

[Is there a way to reset a component?](https://github.com/vuejs/vue/issues/702)

[Proper way to re-initialize the data in VueJs 2.0](https://stackoverflow.com/questions/40340561/proper-way-to-re-initialize-the-data-in-vuejs-2-0)


# Component
+ breaking the app into components

+ Using Vuex to manage states

## Single File Components (a .vue File)
+ < Template >
+ < Script >
+ < Style >


# [Computed Caching vs Methods](https://vuejs.org/v2/guide/computed.html#Computed-Caching-vs-Methods).


Computed properties are cached and will only reevaluate when some of its dependencies have changed.

Method invocations will always run when component re-rendering happens.
which can be expensive if the method functionality performs a lot of computations. 


# https://michaelnthiessen.com/26-time-saving-tips/

