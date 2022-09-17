---
id: 68gxl4kshsqip8aapkwholc
title: JS 1
desc: ''
updated: 1663409164561
created: 1663409164561
isDir: false
title_imported: JS
updated_imported: '2021-04-05T19:10:39.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

[TOC]


















































# [WIP] Javascript pass by value
In JavaScript, **all function arguments are always passed by value**.

It means that JavaScript copies the values of the passing variables into arguments inside of the function.

Any changes that you make to the arguments inside the function does not affect the passing variables outside of the function.

In other words, the changes made to the arguments are not reflected outside of the function.

---
https://stackoverflow.com/questions/518000/is-javascript-a-pass-by-reference-or-pass-by-value-language?answertab=votes#tab-top

```js
function changeStuff(a, b, c)
{
  a = a * 10;
  b.item = "changed";
  c = {item: "changed"};
}

var num = 10;
var obj1 = {item: "unchanged"};
var obj2 = {item: "unchanged"};

changeStuff(num, obj1, obj2);

console.log(num); // 10
console.log(obj1.item); // changed
console.log(obj2.item); // unchanged
```

If `obj1` was not a reference at all, then changing `obj1.item` would have no effect on the `obj1` outside of the function.

If the argument was a proper reference, then everything would have changed. `num` would be `100`, and `obj2.item` would read `"changed"`.

Instead, the situation is that the item passed in is passed by value.

But **the item that is passed by value is itself a reference**. Technically, this is called call-by-sharing.

In practical terms, this means that if you change the parameter itself (as with num and obj2), that won't affect the item that was fed into the parameter. But if you change the internals of the parameter, that will propagate back up (as with obj1).

---

```js
function changeObject(x) {
  x = { member: "bar" };
  console.log("in changeObject: " + x.member);
}

function changeMember(x) {
  x.member = "bar";
  console.log("in changeMember: " + x.member);
}

var x = { member: "foo" };

console.log("before changeObject: " + x.member);
changeObject(x);
console.log("after changeObject: " + x.member); /* change did not persist */

console.log("before changeMember: " + x.member);
changeMember(x);
console.log("after changeMember: " + x.member); /* change persists */

// before changeObject: foo
// in changeObject: bar
// after changeObject: foo

// before changeMember: foo
// in changeMember: bar
// after changeMember: bar
```


It's always pass by value, but for objects the value of the variable is a reference.

Because of this, when you pass an object and change its members, those changes persist outside of the function. This makes it look like pass by reference.

But if you actually change the value of the object variable you will see that the change does not persist, proving it's really pass by value.

---
In other words, the confusing thing here isn't pass-by-value/pass-by-reference.

Everything is **pass-by-value**, full stop.

The confusing thing is that you cannot pass an object, nor can you store an object in a variable. Every time you think you're doing that, you're actually passing or storing a reference to that object.

---

The variable doesn't "hold" the object; it holds a reference.

You can assign that reference to another variable, and now both reference the same object. It's always pass by value (even when that value is a reference...).

---

more info:

https://www.javascripttutorial.net/javascript-pass-by-value/#:~:text=In%20JavaScript%2C%20all%20function%20arguments,variables%20outside%20of%20the%20function.

https://replit.com/@ThinhNguyen19/JS-pass-by-value-argument-function#index.js

https://kentcdodds.com/blog/javascript-pass-by-value-function-parameters?fbclid=IwAR1pcWN2w-B9TWIiS1rC_1GOp6oeXpCLGIAgDsqceLIvCBghC3LqCTuVaUg

https://whimsical.com/JGKp8PrVch7gUxrZ5dsX2A

# [Using String() to safer convert value to String](https://stackoverflow.com/questions/10362114/why-does-stringnull-work)

There is probably just some extra checks and handling for special cases like null and undefined.

[MDN says:](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/String)

It's possible to use String as a "safer" toString alternative, as although it still normally calls the underlying toString, it also works for null and undefined.

# `this` in arrow function

While in ES5 `this` referred to the parent of the function

In ES6, arrow functions use lexical scoping — `this` refers to it’s current surrounding scope and no further. Thus the inner function knew to bind to the inner function only, and not to the object’s method or the object itself.



![68ac5962e589c9bb9e6fb7aadde2c5bf.png](/assets/68ac5962e589c9bb9e6fb7aadde2c5bf-icogpo0coq0s.png)

#  call() - apply() - bind()


```js
.call()
Function.call(value of this, arg1, arg2, …)
//Will execute Function

.apply()
Function.apply(value of this, [arg1, arg2, ..])
//Will execute Function

.bind()
newFunction = Function.bind(value of this, arg1, arg2 …) 
//Will return a new function
```

[JavaScript .call() .apply() and .bind() — explained to a total noob](https://medium.com/@owenyangg/javascript-call-apply-and-bind-explained-to-a-total-noob-63f146684564)

https://repl.it/@ThinhNguyen19/call-in-js


# [Return object literals from arrow functions](https://medium.com/javascript-in-plain-english/how-to-return-object-literals-from-arrow-functions-in-javascript-7c31bfcca8a0)


```js
test1 = () => {
		hello: "world",
    foo: "bar"
	};

// console.log(test1()); // SyxtaxError
```
JavaScript uses curly brackets { } for both block statements and object literals.

The issue with the arrow function is that the parser doesn’t interpret the two braces as an object literal, but as a block statement.


With arrow functions, we need a set of **magic parentheses**:

```js
//add parentheses cover the object
test2 = () => ({
		hello: "world",
    foo: "bar"
	});

console.log(test2()); // { hello: 'world', foo: 'bar'}
```
[repl.it](https://repl.it/@ThinhNguyen19/Return-object-literals-from-arrow-functions)

The magic parentheses force the parser to treat the object literal as an expression instead of a block statement.

When the parser finds parentheses around the entire body of the object literal (an expression), it is not treated as a block statement (not an expression).

According to the ECMAScript specification, block statements cannot be parenthesized, so the opening parenthesis signals the start of an expression.


# Array Method

## includes

```js
let roles = [user];


if (!roles.includes(req.user.role))
	return next ( new AppError/*...*/)
```



## validator snipet
```js
validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
```

## Removing Array Items By Value Using Splice

tốt nhất vẫn là get index và xóa bằng splice cho ngắn gọn

```js
let arr = [1, 2, 3, 4, 5, 5, 6, 7, 8, 5, 9, 0];

for( var i = 0; i < arr.length; i++)
	{ if ( arr[i] === 5) { arr.splice(i, 1); i--; }}
	//=> [1, 2, 3, 4, 6, 7, 8, 9, 0]
```



In the modified example I added 2 additional 5 values to the array.

I also added `i--;` after the splice call.

Now when you execute the loop it will remove every matching item.

If not, as the items are removed from the array the index still increments and the next item after your matched value is skipped.

[@source](https://love2dev.com/blog/javascript-remove-from-array/#remove-from-array-splice-value)

## Non-Enumerable properties In JS - Error instance pro

### Error Instance

JavaScript properties may be non-enumerable, which means they does not appear in for..in loops or Object.keys results or spread the Error object.

When we create an instance of Error in Node, and log it to the console, it just shows err.stack - no err.message

```js
function test1() {
  throw new Error('This is an Error Instance!!!');
}

try {
  test1();
} catch (err) {
  console.log(err);
  console.log(err.stack);
  //output same the err.stack
  // Error: This is an Error Instance!!!
  // at test1 (/home/bastian/myProjects/Node/StudyCode/error.js:2:9)
  // at Object.<anonymous> (/home/bastian/myProjects/Node/StudyCode/error.js:6:3)
  // ...
}
```

but we can access to err.message
```js
 console.log(err.message); //output : This is an Error Instance!!!
 
 console.log(Object.getOwnPropertyNames(err)); // [ 'stack', 'message' ]
 console.log(Object.getOwnPropertyDescriptors(err));
  //
  //   {
  //   stack: {
  //     value: 'Error: This is an Error Instance!!!\n' +
  //       '    at test1 (/home/bastian/myProjects/Node/StudyCode/error.js:2:9)\n' +
  //       '    at Object.<anonymous> (/home/bastian/myProjects/Node/StudyCode/error.js:6:3)\n' +
  //       '    at Module._compile (internal/modules/cjs/loader.js:956:30)\n' +
  //       '    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)\n' +
  //       '    at Module.load (internal/modules/cjs/loader.js:812:32)\n' +
  //       '    at Function.Module._load (internal/modules/cjs/loader.js:724:14)\n' +
  //       '    at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)\n' +
  //       '    at internal/main/run_main_module.js:17:11',
  //     writable: true,
  //     enumerable: false,
  //     configurable: true
  //   },
  //   message: {
  //     value: 'This is an Error Instance!!!',
  //     writable: true,
  //     enumerable: false,
  //     configurable: true
  //   }
  // }

```




You can use Object.getOwnPropertyNames to get all properties (enumerable or non-enumerable) directly on an object.

I say "directly" because normal enumeration looks up the object's prototype chain to get enumerable properties on parent prototypes, while getOwnPropertyNames does not.

Thus, Object.getOwnPropertyNames(err) only shows
```js
['stack', 'message']
```
The name property is a non-enumerable property of `Error.prototype `and is never set directly on an Error instance.
(Prototyping recap: when you try to access err.name, the lookup err turns up nothing, so the interpreter looks at Error.prototype, which does have a name property.)

[@source](https://stackoverflow.com/questions/18277890/why-cant-i-see-the-keys-of-an-error-object)


### Extends Error Class

The same happend with Extends Error Class

```js
class AppError extends Error {
  constructor(message, statusCode) {
    //1st way
    super(message);

    //2nd way
    //super();
    //this.message = message;
  }
}

function test() {
  throw new AppError('Hey this is an App Error!!!', 404);
}

try {
  test();
} catch (err) {

  console.log(err); // err.stack
  console.log(err.message); // Hey this is an App Error!!!
  let myError = { ...err };
  console.log(myError); // {}
}
```

We can acess to `err.message` but cannot clone it using speard operator because message is non-enumerable properties.

Solution:
```js
class AppError extends Error {
  constructor(message, statusCode) {
    //1st way
    //super(message);

    //2nd way
    super();
    this.message = message;
  }
}

function test() {
  throw new AppError('Hey this is an App Error!!!', 404);
}

try {
  test();
} catch (err) {
  let myError = { ...err };
  console.log(myError); // { message: 'Hey this is an App Error!!!' }
}
```

or simply like this:
```js
  let myError = { ...err };
  myError.message = err.message;
  console.log(myError); // { message: 'Hey this is an App Error!!!' }
```

or even better:

Alternate approach: Object.getOwnPropertyNames
> The `Object.getOwnPropertyNames()` method returns an array of all properties (enumerable or not) found directly upon a given object.

Maybe this is the best approach. Object.getOwnPropertyNames gets the name of own object's properties either if they're enumerable or not. That is, you can avoid building the whole propertyNames array, and this approach should be fine with you because you said that all properties aren't enumerable:

```js
var sourceObj = this;
var targetObj = {};    

Object.getOwnPropertyNames(sourceObj).forEach(function(property) {
    targetObj[property] = sourceObj[property]; 
});
```
@source: https://stackoverflow.com/questions/38316864/cloning-non-enumerable-properties-in-javascript

```js
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
  }
}

function test() {
  throw new AppError('Hey this is an App Error!!!', 404);
}

try {
  test();
} catch (err) {

  console.log(err);
  console.log(err.message);
  let myError = { ...err };

  Object.getOwnPropertyNames(err).forEach(function(property) {
    myError[property] = err[property];
  });

  console.log(myError);
  // {
  //   stack: 'Error: Hey this is an App Error!!!\n' +
  //     '    at test (/home/bastian/myProjects/Node/StudyCode/error.js:70:9)\n' +
  //     '    at Object.<anonymous> (/home/bastian/myProjects/Node/StudyCode/error.js:74:3)\n' +
  //     '    at Module._compile (internal/modules/cjs/loader.js:956:30)\n' +
  //     '    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)\n' +
  //     '    at Module.load (internal/modules/cjs/loader.js:812:32)\n' +
  //     '    at Function.Module._load (internal/modules/cjs/loader.js:724:14)\n' +
  //     '    at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)\n' +
  //     '    at internal/main/run_main_module.js:17:11',
  //   message: 'Hey this is an App Error!!!'
  // }
```

It's weird when `console.log(err)` didn't show message but when we clone it, it showed it on console.

Something to dig deeper:
```js
  // console.log(err.hasOwnProperty('message'));
  // console.log('---------------------');
  // console.log('Error');
  // console.log(Object.getOwnPropertyNames(Error));
  // console.log(Object.getOwnPropertyDescriptors(Error));
  // console.log(Object.getOwnPropertyNames(Error.prototype));
  // console.log(Object.getOwnPropertyDescriptors(Error.prototype));
```

### External resource:
**The problem:**
https://repl.it/@ThinhNguyen19/Test-Error-Class-Node-JS
https://stackoverflow.com/questions/54550955/how-to-get-property-out-of-javascript-object-which-is-only-appearing-in-console

https://stackoverflow.com/questions/42283600/js-why-message-property-of-error-is-non-enumerable

https://stackoverflow.com/questions/51677276/error-object-property-can-not-be-iterated-by-for-loop


**Cloning**
https://stackoverflow.com/questions/38316864/cloning-non-enumerable-properties-in-javascript
https://stackoverflow.com/questions/8024149/is-it-possible-to-get-the-non-enumerable-inherited-property-names-of-an-object
https://stackoverflow.com/questions/38416020/deep-copy-in-es6-using-the-spread-syntax

**Custom Errors**
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error
https://javascript.info/custom-errors
https://medium.com/learn-with-talkrise/custom-errors-with-node-express-27b91fe2d947

**Dig Deeper**
https://javascript.info/class-inheritance
https://medium.com/@akash.mihul/understanding-inheritance-and-prototype-chain-in-javascript-75d0086fd90f
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty



## Using Console.log effectively

Đóng gói các biến thành object và sử dụng cú pháp khai báo rút gọn của ES6
```js
const foo = { name: "Alex", age: "26", gender: "male" };
const bar = { name: "Jenifer", age: "18", gender: "female" };
const baz = { name: "John", age: "30", gender: "male" };

console.log({ foo, bar, baz });

/*
{
 bar: {name: "Jenifer", age: "18", gender: "female"},
 baz: {name: "John", age: "30", gender: "male"},
 foo: {name: "Alex", age: "26", gender: "male"},
}
*/
[@source](https://completejavascript.com/thu-thuat-su-dung-console-hieu-qua)
```


## Convert String to number
The  `parseInt()`  method converts a string into an integer (a whole number).

It accepts two arguments. The first argument is the string to convert. The second argument is called the  `radix`. This is the base number used in mathematical systems. For our use, it should always be  `10`.

```js
var text = '42px';
var integer = parseInt(text, 10);
// returns 42
```

## Measure time taken

 The best solution is using  [**performance.now()**](https://developer.mozilla.org/en-US/docs/Web/API/Performance.now): 
```js
const { performance } = require('perf_hooks'); // // NodeJs: it is required to import the performance class

let t0 = performance.now();

// doSomething();   // <---- The functions you're measuring time for 

let t1 = performance.now();
console.log("Took " + (t1 - t0) + " milliseconds.");
```
[@source](https://stackoverflow.com/questions/313893/how-to-measure-time-taken-by-a-function-to-execute)

## Expressions and Operators

#### [Spread Synxtax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

Spread in array literals
```js
    var parts = ['shoulders', 'knees']; 
    var lyrics = ['head', ...parts, 'and', 'toes']; 
    // = ["head", "shoulders", "knees", "and", "toes"]
```
Just like spread for argument lists, ... can be used anywhere in the array literal and it can be used multiple times.

## Copy an array ( shallow clone )
```js
    var arr = [1, 2, 3];
    var arr2 = [...arr]; // like arr.slice()
    arr2.push(4); 
    
   // arr2 becomes [1, 2, 3, 4]
   // arr remains unaffected
```
Spread syntax effectively goes one level deep while copying an array.
Therefore, it may be unsuitable for copying multidimensional arrays as the following example shows (it's the same with Object.assign() and spread syntax).
```js
    var a = [[1], [2], [3]];
    var b = [...a];
    b.shift().shift(); // 1
    // Now array a is affected as well: [[], [2], [3]]
```
## A better way to concatenate arrays

[Array.prototype.concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) is often used to concatenate an array to the end of an existing array.
*Without spread syntax this is done as:*
```js
    var arr1 = [0, 1, 2];
    var arr2 = [3, 4, 5];
    // Append all items from arr2 onto arr1
    arr1 = arr1.concat(arr2);
```
*With spread syntax this becomes:*
```js
    var arr1 = [0, 1, 2];
    var arr2 = [3, 4, 5];
    arr1 = [...arr1, ...arr2]; // arr1 is now [0, 1, 2, 3, 4, 5]
```

[Array.prototype.unshift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) is often used to insert an array of values at the start of an existing array.
*Without spread syntax this is done as:*
```js
    var arr1 = [0, 1, 2];
    var arr2 = [3, 4, 5];
    // Prepend all items from arr2 onto arr1
    Array.prototype.unshift.apply(arr1, arr2) // arr1 is now [3, 4, 5, 0, 1, 2]
```
*With spread syntax, this becomes:*
```js
    var arr1 = [0, 1, 2];
    var arr2 = [3, 4, 5];
    arr1 = [...arr2, ...arr1]; // arr1 is now [3, 4, 5, 0, 1, 2]
```

**Note**: Unlike `unshift()`, this creates a new `arr1`, and does not modify the original `arr1` array in-place.


## Understanding Method Chaining In Javascript
`this` holds the key to chaining methods. When we create an object using an expression such as  `object = new SomeClassWeCreated()`, and call the methods defined on that object,  `this`  refers to that object. 

 `this`  is the current object instance, therefore, it has access to all the methods defined on the instance as well as access to the  [prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype). So, if we wanted to access a method defined on the current object instance (which  `this`  refers to), we can write  `this.someMethod()`. Cool, yeah?

```js
class ChainAble {
  firstMethod() {
    console.log('This is the First Method');
    return this;
  }
  
  secondMethod() {
    console.log('This is the Second Method');
    return this;
  }
  
  thirdMethod() {
    console.log('This is the Third Method');
    return this;
  }
}

const chainableInstance = new Chainable()
chainableInstance
  .firstMethod()
  .secondMethod()
  .thirdMethod();

// Console Output
// This is the First Method
// This is the Second Method
// This is the Third Method
```
[link text](#the-header)

Next step is to find a way to make sure that we can call another method on the return value of the  `this.someMethod()`, so we can have  `this.someMethod().someOtherMethod()`. To achieve this, we simply tell our methods to return  `this`. That way, each method returns the object that contains the methods we want.

Okay, that’s all nice and good. Now we can chain our methods and produce some side-effects — logging to the console in this case. Our code is a lot more readable and clean, but how do we get actual values from our chainable class?

### Instance Properties

To get the result of our chain of function calls, we need to store the results of each function call and then access that data at the end of our chain. For this, we’ll add an instance property to our class to hold the result of each function/method call.

Instance properties are properties unique to each object instance, meaning if we create two objects(instances) from the same class by writing  `new SomeClassWeWrote()`, their values can be manipulated independently. Since  `this`  refers to the current object instance, attaching the property to  `this`  e.g  `this.property = ‘someValue'`  guarantees it’ll be available to the current instance and not shared with other instances of the same class.

```js
class Arithmetic {
  constructor() {
    this.value = 0;
  }
  get val() {
    return this.value;
  }
  sum(...args) {
    this.value = args.reduce((sum, current) => sum + current, 0);
    return this;
  }
  add(value) {
    this.value += value;
    return this;
  }
  subtract(value) {
    this.value -= value;
    return this;
  }
  average(...args) {
    this.value = args.length
      ? (this.sum(...args).value) / args.length
      : undefined;
    return this;
  }
}

a = new Arithmetic()
a.sum(1, 3, 6)   // => { value: 10 } 
  .subtract(3)   // => { value: 7 }
  .add(4)        // => { value: 11 }
  .val           // => 11 

// Console Output
// 11
```

[@source](https://medium.com/backticks-tildes/understanding-method-chaining-in-javascript-647a9004bd4f)


### Dig deeper

[https://repl.it/@ThinhNguyen19/test-chaining-method](https://repl.it/@ThinhNguyen19/test-chaining-method)

[https://stackoverflow.com/questions/6589497/callback-function-use-of-parentheses](https://stackoverflow.com/questions/6589497/callback-function-use-of-parentheses)

```js

class Arithmetic {
  constructor() {
    this.value = 0;
  }

  get val() {
    return this.value;
  }

  add(value = 0) {
    this.value += value;
    return this;
  }

  subtract(value = 0) {
    this.value -= value;
    return this;
  }
}

let a = new Arithmetic();
//a.subtract(3).add(4).val;

console.log(a.add(30)); // Arithmetic { value: 6 }

class MainFn {
  constructor(obj) {
    this.obj = obj;
  }

  addthem(value) {
    this.obj.value += value;
    return this;
  }

  subtract(value) {
    this.obj.value -= value;
    return this;
  }
}

let b = new MainFn(a.add(30)).addthem(40).obj.val;

console.log(b); //81

```


# Synchronous Asynchronous
Asynchronous function are functions that we can execute later.

> JS is  single threaded - only one statement is executed at a time.

Synchronous tasks are do from top to bottoms.
When we have to do heavy tasks like image processing or making requests over the network like API call.
Everything must stop to wait to that tasks. It's not good.

So we need Asynchronous task, where it can run and return the result later, and at that time, JS can do another task.
It's non-blocking.
 
![enter image description here](http://i.imgur.com/aB4NCfg.png)

## Promises
> A promise is an object that may produce a single value *not now but later, some time in the future*
> Either a *resolved* value, or a reason it's not resolved or *rejected*.

A promise has 3 states :
+ pending
+ fulfilled
+ rejected

Instead of using callbacks, we now had a native way to handle asynchronous code using *promises*

### Job Queues

![enter image description here](http://i.imgur.com/xwnv5uR.png)

 ```js
 //callback Queue - Task Queue - Web API - not native JS code
setTimeout(()=>{console.log('1', 'is the loneliest number')}, 0)
setTimeout(()=>{console.log('2', 'can be as bad as one')}, 10)

//2 - Job Queue - Microtask Queue - Native JS - higher priority - Event Loop will check first before check call back Queue
Promise.resolve('hi').then((data)=> console.log('2', data))

//3
console.log('3','is a crowd')

// OUTPUT
// 3 is a crowd
// 2 hi
// => undefined // 
// 1 is the loneliest number
// 2 can be as bad as one
```
[@repl.it](https://repl.it/@ThinhNguyen19/async)

[Chrome/Firefox console.log always appends a line saying undefined](https://stackoverflow.com/questions/14633968/chrome-firefox-console-log-always-appends-a-line-saying-undefined)


## Async Await - ES8
### External Resource
https://ehkoo.com/bai-viet/tat-tan-tat-ve-promise-va-async-await

https://kipalog.com/posts/JS--async-await-don-gian

### Sequence Promise



### Parallel, Sequence and Race

```js
var t0 = performance.now();

const promisify = (item, delay) =>
  new Promise((resolve) =>
    setTimeout(() =>
      resolve(item), delay));

const a = () => promisify('a', 100);
const b = () => promisify('b', 5000);
const c = () => promisify('c', 3000);

const functionA = async () =>  await a()
async function aA() {
  return await a();
}
const functionB = async () =>  await b()
const functionC = async () =>  await c()
  

async function parallel() {
  const promises = [a(), b(), c()];
  const [output1, output2, output3] = await Promise.all(promises);
  return `prallel is done: ${output1} ${output2} ${output3}`
}

async function race() {
  const promises = [a(), b(), c()];
  const output1 = await Promise.race(promises);
  return `race is done: ${output1}`;
}

async function sequence() {
  const output1 = await a();
  const output2 = await b();
  const output3 = await c();
  return `sequence is done ${output1} ${output2} ${output3}`
}

aA().then(data => console.log(`Still a but write before`,data,performance.now()-t0)); // 1st

sequence().then(data => console.log(data,performance.now()-t0)); // 7th
parallel().then(data => console.log(data,performance.now()-t0)); // 5th
race().then(data => console.log(data,performance.now()-t0)); // 2nd


functionA().then(data => console.log(`still a but write after`,data,performance.now()-t0)); // 3rd
functionB().then(data => console.log(data,performance.now()-t0)); // 6th
functionC().then(data => console.log(data,performance.now()-t0)); // 4th

// OUTPUT
// Still a but write before a 116.30499999955646
// race is done: a 117.88499999966007
// still a but write after a 118.49999999867578
// c 3000.7299999997485
// prallel is done: a b c 5000.495000000228
// b 5001.694999999017
// sequence is done a b c 8118.580000000293
```




[https://repl.it/@ThinhNguyen19/async-1](https://repl.it/@ThinhNguyen19/async-1)


   
@source: Andrei Neagoie - Advanced JavaScript Concepts > 9. Asynchronous JavaScript




