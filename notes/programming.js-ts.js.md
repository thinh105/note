---
id: tmzx4dcyylky0si1tn2fmjc
title: JS
desc: ''
updated: 1663409164561
created: 1663409164561
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: JS
updated_imported: '2022-05-30T04:07:11.000Z'
created_imported: '2019-10-30T16:58:00.000Z'
---

[TOC]




























# Closures
>[Clousure] makes it possible for a function to have "private" variables.
> 
> @W3School

## Refs:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

https://javascript.info/closure


 
# IFFE - Immediately Invoked Function Expression

## Theory
IIEFs are well-known for protecting variables scope.

A variable defined in an **IIFE** can’t be accessed from the outside.

Or when you use let or const to declare a variable, it can only be accessed in the enclosing block.

## Practical Use cases:

### Closures with IFFE to protect private data

**Closures** give you the ability to access an outer function’s scope from an inner function. Creating a **closure** is nothing but defining a function inside another function and expose it.

When combining a closure with an IIFE, you get two great benefits:
1. The scopes of variables are secured to prevent unexpected behaviors.
2. You can modify variables inside a function from the outside. It sounds like we are breaking the first benefit. Except we don’t. Because you can’t modify the variables’ values directly but only from an exposing function. And it’s safe.

```js
const user = (function() {
  let name = ‘anonymous’; // private
  return {
    getName: _ => name,
    setName: newName => name = newName
  };
})();
console.log(user.getName()) // anonymous
user.setName(‘Amy’);
console.log(user.getName()); // Amy
```


### Alias Global Variables
Using many JavaScript libraries can cause conflicts because some of them might export an object with the same name

Fortunately, you can use IIFEs to solve this problem by applying the aliasing technique:
```js
(function ($) {
  // You’re safe to use jQuery here
})(jQuery);
```
By wrapping your code inside an IIFE that takes jQuery as an argument, we will make sure that the $ symbol now refers to jQuery, not other libraries.

### Secure Variables Scope ( Just useful before ES6 - `let` and `const`  come to solve this problem)

what happens in the IIFE scope, stays in the IIFE scope. You can’t use the variable defined inside IIFE from the outside.

```js
(function () {
  var greeting = ‘Good morning! How are you today?’;
  console.log(greeting); // Good morning! How are you today?
})();
console.log(greeting); // error: Uncaught ReferenceError: greeting is not defined
```

The classic interview question about the difference between using the `for loop` with `var` and `let` 

## Ref:
https://javascript.plainenglish.io/4-practical-use-cases-for-iifes-in-javascript-6481dcb0ba7d

## Read more:
https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var

https://stackoverflow.com/questions/750486/javascript-closure-inside-loops-simple-practical-example



# Clone


# Observable

Observables are functions that throw values. Objects called observers subscribe to these values. Observables create a pub-sub system based on the observable design pattern. This makes observables popular with async programming in modern JavaScript frameworks like Angular and libraries like React.

Unlike Promises, observables are not yet inherit to JavaScript. This is why Angular and React rely on the RxJS library for implementing observables. RxJs stands for "Reactive Extension for JavaScript". The RxJS library defines its own Observable class along with supporting methods for Reactive programming.

[@](https://www.stackchief.com/tutorials/JavaScript%20Observables%20in%205%20Minutes)

[Promises vs Observable](https://medium.com/@mpodlasin/promises-vs-observables-4c123c51fe13)

# Partial merge object in CW

![ea98c4eb24eae1b1f18e0682434465e1.png](/assets/ea98c4eb24eae1b1f18e0682434465e1-jkti7w1flytp.png)

# Promise

Promise, new in ES6, are objects that represent the not-yet-available result of an asynchronous operation.

## Promise constructor

The constructor syntax for a promise object is:

```js
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```

- The Executor
    
    - The function passed to new Promise is called the executor.
    - When new Promise is created, the executor runs automatically.
    - It contains the producing code which should eventually produce the result
- Its arguments `resolve` and `reject` are callbacks provided by JavaScript itself. Our code is only inside the executor.
    

When the executor obtains the result, be it soon or late, doesn’t matter, it should call one of these callbacks:

resolve(value) — if the job is finished successfully, with result value. reject(error) — if an error has occurred, error is the error object.

## Resource:

https://javascript.info/promise-basics \[ebook\] JavaScript_ The Definitive Guid - David Flanagan.pdf

# Async await

## Await in top level

You can use the await keyword on its own (outside of an async function) within a JavaScript module. This means modules, with child modules that use await, wait for the child module to execute before they themselves run. All while not blocking other child modules from loading.

Here is an example of a simple module using the Fetch API and specifying await within the export statement. Any modules that include this will wait for the fetch to resolve before running any code.

```js
// fetch request
const colors = fetch('../data/colors.json')
  .then(response => response.json());

export default await colors;
```

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await

not working snippet code: https://replit.com/@ThinhNguyen19/test-async-await#index.js

commonJS vs ModulesJS
https://stackoverflow.com/questions/38296667/getting-unexpected-token-export

https://blog.logrocket.com/commonjs-vs-es-modules-node-js/#:~:text=ES%20modules%20are%20the%20standard,encapsulating%20JavaScript%20code%20for%20reuse.

## when not to use await ?

### When you dont have to return any 
Eslint has a rule for [Disallows unnecessary return await (no-return-await)](https://eslint.org/docs/rules/no-return-await)

Using return await inside an async function keeps the current function in the call stack until the Promise that is being awaited has resolved, at the cost of an extra microtask before resolving the outer Promise. `return await` can also be used in a `try/catch` statement to catch errors from another function that returns a Promise.

You can avoid the extra microtask by not awaiting the return value, with the trade off of the function no longer being a part of the stack trace if an error is thrown asynchronously from the Promise being returned. This can make debugging more difficult.

[`async function` on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

[`await vs return vs return await` by Jake Archibald](https://jakearchibald.com/2017/await-vs-return-vs-return-await/)

### Further Reading

[How to use Async and Await with Array.prototype.map()](https://flaviocopes.com/javascript-async-await-array-map/)

# [JavaScript Visualized series](https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif)

![08cd4b96f375d2793d7102cee2384fc1.png](/assets/08cd4b96f375d2793d7102cee2384fc1-mkk07z2yd9dr.png)

# what is Factory Function ?

Fuction that returns another function.

**handlerFactory.js**

```js
//outer function

const catchAsync = require('../utils/catchAsync');
const AppError = require('../utils/appError');

// inner function
exports.deleteOne = Model =>
  catchAsync(async (req, res, next) => {
    const doc = await Model.findByIdAndDelete(req.params.id);

    if (!doc)
      return next(new AppError('No document found with that ID!!!', 404));

    // in RESTful API, common practice is not send anything back to client when deleted
    res.status(204).json({
      status: 'success',
      data: null
    });
  });
```

**tourController.js**

```js
const factory = require('./handlerFactory');

/*...*/

exports.deleteTour = factory.deleteOne(Tour);
```

The factory function will get the `Model` as argument and return the `delete` fuction for the `tourController.js` This works because of JS closures, that inner function will get access to the variable of outer function even after the outer has already returned.

The return function will sit in tourController and wait until it is finally called when we hit the corresponding route.

# What is Object liretal ?

```js
let newObjLiteral = { ... }; // object literal

let newObjNonLiteral = new Object ();
// object non-literal using Object constructor
```

# 5 Uses for the Spread Operator

```js
// Copying an array
let arr = [1,2,3,4]
let copy = [...arr] // copy is [ 1, 2, 3, 4 ]
let wrongCopy = [arr] // wrongCopy is [ [1, 2, 3, 4] ]


// Concatenate arrays
let arr1 = [1,2,3,4]
let arr2 = [5,6,7,8]
let concat = [...arr1, ...arr2]
// concat is [ 1, 2, 3, 4, 5, 6, 7, 8 ]



//Pass arguments as arrays
function dev(x, y, z) { }
var args = [0, 1, 2]
dev(...args) // call function


//Copy an Obj
let obj = {a: 1, b: 2, c: 3}
let copy = {...obj}
// copy is {a: 1, b: 2, c: 3}
let wrongCopy = {obj}
// wrongCopy is { {a: 1, b: 2, c: 3} }


//Merge object
let obj1 = {a: 1, b: 2, c: 3}
let obj1 = {d: 4, e: 5, f: 6}

let merge = {...obj1, ...obj2}
// merge is {a: 1, b: 2, c: 3, d: 4, e: 5, f: 6}

```

# Spread Synxtax `...` in ...mapGetters, ...mapActions, ...mapState

Spread Synxtax `...` using to merge getter properties with your local computed propeties.

We could actually just do `computed: mapState(['todo'])` or `computed: Vuex.mapGetters` but than you couldn't have local computed properties.

By using the three dots (spread operator) (wrapped in curly braces you include all props of mapState in your object and can then define additional local properties. So it's merging both objects kind of like Object.assign()

## Spread in object literals (MDN)

It copies own enumerable properties from a provided object onto a new object.

Shallow-cloning (excluding prototype) or merging of objects is now possible using a shorter syntax than Object.assign().

```js
const obj1 = { foo: 'bar', x: 42 };
const obj2 = { foo: 'baz', y: 13 };

const clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

const mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 } - update the foo's value
```

[@MDN](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

[@SOF](https://stackoverflow.com/questions/39854956/three-periods-syntax-in-vuex)

[@Vuex](https://vuex.vuejs.org/guide/state.html)

# External Resource

Modern JS Cheatsheet: https://github.com/mbeaudru/modern-js-cheatsheet

https://github.com/ryanmcdermott/clean-code-javascript

http://jstherightway.org/