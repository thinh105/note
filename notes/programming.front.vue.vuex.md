---
id: qi3iktt8sz15wtjzkp67mcq
title: Vuex
desc: ''
updated: 1663409164564
created: 1663409164564
isDir: false
latitude: 15.8794
longitude: 108.335
altitude: 0
title_imported: Vuex
updated_imported: '2020-08-14T06:09:12.000Z'
created_imported: '2020-06-28T15:05:24.000Z'
---

[toc]












------


# Handling Authentication In Vue Using Vuex

https://scotch.io/tutorials/handling-authentication-in-vue-using-vuex


# Error in Vue/Vuex

## [API error handling in vue with axios](https://www.qcode.in/api-error-handling-in-vue-with-axios/)


## Display error: [Centralized alert for Vuetify](https://techformist.com/centralize-alert-vuetify/)

## Handling error in async await code without try/catch


### [NPM package await-to-js | How to write async await without try-catch blocks in Javascript ](https://blog.grossman.io/how-to-write-async-await-without-try-catch-blocks-in-javascript/)

https://github.com/scopsy/await-to-js

### [Better error handling with async/await](https://dev.to/sobiodarlington/better-error-handling-with-async-await-2e5m)

### 
https://medium.com/locale-ai/architecting-http-clients-in-vue-js-applications-for-efficient-network-communication-991cf1df1cb2



# Explain Vuex visually 

https://vuex.vuejs.org/

# [Practical usage of Action.type.js and Mutations.type.js in Vuex](https://stackoverflow.com/questions/51268001/practical-usage-of-mutation-types-in-vuex)


## [@Vuex Docs : Using Constants for Mutation Types](https://vuex.vuejs.org/guide/mutations.html#using-constants-for-mutation-types)

It is a commonly seen pattern to use constants for mutation types in various Flux implementations. This allows the code to take advantage of tooling like linters, and putting all constants in a single file allows your collaborators to get an at-a-glance view of what mutations are possible in the entire application:


**Example: **

```js

// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'

// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // we can use the ES2015 computed property name feature
    // to use a constant as the function name
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```
Or in RealWorld Vue app

Whether to use constants is largely a preference - it can be helpful in large projects with many developers, but it's totally optional if you don't like them.


## Answer in SOF


# [Getters](https://vuex.vuejs.org/guide/getters.html)

## Method-Style Access
You can also pass arguments to getters by returning a function. This is particularly useful when you want to query an array in the store:
```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```
.

```js
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

Note that getters accessed via methods will run each time you call them, and the result is not cached.

## [MDN Returning a function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/return)

```js
function magic() {
  return function calc(x) { return x * 42; };
}

var answer = magic();
answer(1337); // 56154
```

Read more: [Closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)


## [Demonstration pass argument to getters by returning a function](https://repl.it/@ThinhNguyen19/Higher-Order-Function#index.js)

```js
// HOF

// ex1

const state1 = 3;
let hof = () => (id)=> id*5;
let abc = hof(); // = (id) => id * 5  
console.log('ex1', abc(3)); //15  // = (3) => 3 * 5 = 15


//ex2
const state = 3;
let hof = (state) => (id)=> id*state;

let abc = hof(state);

console.log('ex2',abc(7));  //21


//ex3
const state = 3;

let hof = (state) => ()=> state*10;

let abc = hof(state);
abc();  //30


// ex4

let store = {
  state: {
   data: 3
  },

  getters: {
    hof: function(state){
         return state.data
       } 
    } 
}


// Vuex auto pass the state argument behind the scene 
let vuexGetter = store.getters.hof(store.state);
// =================================================


//The getters will be exposed on the store.getters object, and you access values as properties:
vuexGetter; //3


// ex5

let store = {
  state: {
   data: 3
  },


// You can also pass arguments to getters by returning a function. This is particularly useful when you want to query an array in the store:
  getters: {
    hof: state => id => state.data * id
  } 
} 

// Vuex auto pass the state argument behind the scene 
let vuexGetter = store.getters.hof(store.state);
// =================================================


vuexGetter(3); //3

```




# Validate your forms the easy way — using Vuelidate

# 2 way to use Form Handling in Vuex | Two-way Computed Property
Admittedly, the above is quite a bit more verbose than v-model + local state, and we lose some of the useful features from v-model as well. An alternative approach is using a two-way computed property with a setter:
```js
<input v-model="message">
// ...
computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    }
  }
}
```
[Docs](https://vuex.vuejs.org/guide/forms.html)

# Understanding ...mapGetters in Vuex
We can use directly mapGetters as: computed: mapGetters([/*...*/] without Spread Syntax ... when you don't have any local computed properties.
```js
computed: {
//nothing here - no any local computed properties
      ...mapGetters(['cartItems', 'cartTotal', 'cartQuantity']),
    },

computed: mapGetters(['cartItems', 'cartTotal', 'cartQuantity']),
```

Both of above do exactly the same thing!

But when you do have any local computed property, then you need Spread Syntax. It because the `mapGetters` returns an object.

And then we need Object Spread Operator to merge multiple Objects into one.
```js
computed: {
  localComputed () { /* ... */ },
  // we use ... Spread Operator here to merge the local object with outer objects
  ...mapGetters(['cartItems', 'cartTotal', 'cartQuantity']),
}
```

The same happens to `mapActions`, `mapState`. 

You can read more about Spread in object literals in MDN

Basically, in this situation, it used to merge objects
```js
let obj = {a: 1, b: 2, c: 3}
let copy = {...obj}
// copy is {a: 1, b: 2, c: 3}

//without ..., it will become wrong
let wrongCopy = {obj}
// wrongCopy is { {a: 1, b: 2, c: 3} } - not what you want
```
# Vuex’s key ideas

Those ideas are:

- All of our application’s data is in a single data structure called the state, which is held in the
store
- Our app reads the state from this store
- The state is never mutated directly outside the store
- The views dispatch actions that describe what happened
- The actions commit to mutations
- Mutations directly mutate/change store state
- When the state is mutated, relevant components/views are re-rendered


# A Vuex store

The heart of a Vuex implementation is the Vuex store.\
The store is where the application data, (i.e. state) is kept.\
**State** can never be mutated directly and can only be modified by calling **mutations**.\
**Actions** are often responsible in calling **mutations** and are themselves dispatched within components.\
A Vuex store also allows us to define **getters**, methods that involve receiving computed state data.

![897dd2fd9cf0c1665d2d68886250828c.png](/assets/897dd2fd9cf0c1665d2d68886250828c-zlvjx7zs9vnf.png)

# State
Application level data is the data that needs to be shared between components;
```js
const state = {
notes: [],
timestamps: []
}
```
# Mutations
In Vuex, mutations need to be explicitly defined. A mutation consists of a string type and a handler.\
In Flux architectures, mutation string types are often declared in **CAPITAL LETTER** to distinguish them from other functions and for tooling/linting purposes.

The first argument passed in is the state!

**Mutations always have access to state as the first argument**

The payload is an optional argument and, in some cases we can safely ignore it. However, in our case the mutations need access to the new note and timestamp objects to update the state arrays.
These data objects will therefore be passed as the second argument to each mutation function.

```js
const mutations = {
//When the mutation function is run,
//the first argument passed in is the state.
reiterate that mutations always have access to state as the first argument
	ADD_NOTE (state, payload) {
		state.notes.push(payload);
	},
	ADD_TIMESTAMP (state, payload) {
		state.timestamps.push(payload);
	}
}
```

It’s important to remember that mutations have to be synchronous.\
If asynchronous tasks need to be done, actions are responsible in dealing with them prior to calling mutations.

# Actions
Actions are functions that exist to call mutations.
In addition, actions can perform asynchronous calls/logic handling before committing to mutations.

```js
const actions = {
	addNote (context, payload) {
		context.commit('ADD_NOTE', payload);
	},
	addTimestamp (context, payload) {
		context.commit('ADD_TIMESTAMP', payload);
	}
}
```

# Getters
Getters are to an application store what computed properties are to a component. Getters are used to derive computed information from store state.
We can call getters multiple times in our actions and in our components

```js
const getters = {
	getNotes: state => state.notes,
	getTimestamps: state => state.timestamps,
	getNoteCount: state => state.notes.length
}
```

# Store
With the state, mutations, actions, and getters all set-up, the final part of integrating Vuex into our application is creating and integrating the store.

Creating the store means we’ll need to wire everything together.

The store object is how our **mutations** and **getters** get access to the **state** object and how **actions** can directly commit to **mutations**
with `context.commit`.

```js
const store = new Vuex.Store({
state,
mutations,
actions,
getters
})
```

# Inject the store to App
To inject the store to the entire application and have it accessible within all components, we need to
pass the store object to the application’s Vue instance:
IV - Introduction to Vuex 136
```js
new Vue({
	el: '#app',
	store,
	components: {
	'input-component': inputComponent
	}
})
```

# Building the components

Store actions are dispatched simply with `this.$store.dispatch ('nameOfAction', payload)`

```js
const inputComponent = {
  template: `<input placeholder='Enter a note' v-model="input" @keyup.enter="monitorEnterKey" class="input is-small" type="text" />`,
  data() {
    return {
      input: '',
    };
  },
  methods: {
    monitorEnterKey() {
    //call the action
      this.$store.dispatch('addNote', this.input);
      this.$store.dispatch('addTimestamp', new Date().toLocaleString());
      this.input = '';
    },
  },
};

const noteCountComponent = {
  template: `<div class="note-count">
            Note count: <strong>{{noteCount}}</strong>
            </div>`,
  computed: {
    noteCount() {
    // call the getter
      return this.$store.getters.getNoteCount;
    },
  },
};
```

# Vuex Modules


```js
const moduleOne = {
stateOne,
mutationsOne,
actionsOne,
gettersOne
}
const moduleTwo = {
stateTwo,
mutationsTwo,
actionsTwo,
gettersTwo
}
// When the store is instantiated,
// the module objects can then be introduced to the store’s modules property:

const store = new Vuex.store({
modules: {
moduleOne,
moduleTwo
}
});
```

# Keep the Vuex and server state in sync


With a portion of our Vuex store complete, we can now make the ProductList component dynamic.

For us to retrieve information from the store, we first need to invoke the mutation that syncs the server data with the store. 

Since we need the mutation to occur at the moment the component is created, we’ll dispatch the getProductItems action within the component’s created() hook.

In the < script > tag of the ProductList.vue file, we’ll set up the created() hook to dispatch the `getProductItems` action. Since the store is injected through the entire application, we can access it with this.$store:

```js
<script>
export default {
name: 'ProductList',
created() {
this.$store.dispatch('getProductItems');
}
}
</script>
```




