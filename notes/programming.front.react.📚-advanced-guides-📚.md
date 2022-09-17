---
id: irocyf2nxm70xbp43cesbrz
title: "\U0001F4DA Advanced Guides \U0001F4DA"
desc: ''
updated: 1663409164564
created: 1663409164564
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: "\U0001F4DA ADVANCED GUIDES \U0001F4DA"
updated_imported: '2022-08-24T17:59:25.000Z'
created_imported: '2022-01-07T16:45:56.000Z'
---

[TOC]


















































# Performance
https://kentcdodds.com/blog/fix-the-slow-render-before-you-fix-the-re-render


# State management in React
https://kentcdodds.com/blog/application-state-management-with-react

https://kentcdodds.com/blog/how-to-use-react-context-effectively

# HOCs

https://www.robinwieruch.de/react-higher-order-components/



# [Hook FAQS](https://reactjs.org/docs/hooks-faq.html)

# React Hooks Closure Problem
Closures are one of the reasons it’s important to keep dependency lists correct.
- If the concept of a “closure” is new or confusing to you, [then give this a read](https://mdn.io/closure).


## [Why am I seeing stale props or state inside my function?](https://reactjs.org/docs/hooks-faq.html#why-am-i-seeing-stale-props-or-state-inside-my-function)

![316bfd83b8e23503f3b86c87b40185e2.png](/assets/316bfd83b8e23503f3b86c87b40185e2-cyctcnqb9oel.png)

If you intentionally want to read the latest state from some asynchronous callback, you could keep it in a ref, mutate it, and read from it.
![0de2be3e3a3a4b20711ed812321343bc.png](/assets/0de2be3e3a3a4b20711ed812321343bc-1vo6ri725m53.png)

## More detail:

- [my sandbox](https://codesandbox.io/s/friendly-water-cctss?file=/src/index.js)
- [Demostration React Hooks Closure Problems by Ben Awad](https://www.youtube.com/watch?v=eTDnfS2_WE4)


If you first click “Show alert” and then increment the counter several times, the alert will show the count variable at the time you clicked the “Show alert” button.

- How React Uses Closures to Avoid Bugs
	- https://epicreact.dev/how-react-uses-closures-to-avoid-bugs/


## Ref

- https://www.netlify.com/blog/2019/03/11/deep-dive-how-do-react-hooks-really-work/

- https://overreacted.io/how-are-function-components-different-from-classes/


## [Implement React - Getting Closure on React Hooks by Shawn Wang | JSConf.Asia 2019](https://www.youtube.com/watch?v=KJP1E-Y-xyo)



# [Composition](https://reactjs.org/docs/composition-vs-inheritance.html)

Some components don’t know their children ahead of time. This is especially common for components like Sidebar or Dialog that represent generic “boxes”.

We recommend that such components use the special children prop to pass children elements directly into their output:

```js
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```
This lets other components pass arbitrary children to them by nesting the JSX:
```js
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```


# Prop drilling
Prop drilling (also called "threading") refers to the process you have to go through to get data to parts of the React Component tree.

https://kentcdodds.com/blog/prop-drilling

## [Using Composition in React to Avoid "Prop Drilling" - Michael Jackson](https://www.youtube.com/watch?v=3XaXKiXtNjw)
	
> All that junk you're putting in React context should just be props. Legit uses for 
>context are rare, and mostly for library code, not your app

![3477abf39a41c1c1dfffbfbd14b8df91.png](/assets/3477abf39a41c1c1dfffbfbd14b8df91-t0q89vpb8zfz.png)

The idea is instead of Props Drilling or using Context, we can use React composition or React children to insert the CurrentUser only in the component need it,

But seem like many disagree on the comments, since this example is quite easy, not reflect the complexity in realworld problem.

> Lee Robinson 2 years ago
>
>For simple examples, I think this works really well. I'd argue for larger examples Context would work better. The solution with composition ends up creating a larger JSX tree at the top level, which again, is fine while it's small. The problem compounds as you continue to get components of components of components.
>
> I think in an ideal world there's a little bit of both. But I agree with what you're saying in theory - you don't need global state to avoid prop drilling.

# Clone In React

When you deal with deeply nested objects, updating them in an immutable way can feel convoluted. If you run into this problem, check out [Immer](https://github.com/mweststrate/immer) or [immutability-helper](https://github.com/kolodny/immutability-helper).

These libraries let you write highly readable code without losing the benefits of immutability.

### [The Main Idea](https://github.com/kolodny/immutability-helper#the-main-idea)

If you mutate data like this:

```js
myData.x.y.z = 7;
// or...
myData.a.b.push(9);
```

You have no way of determining which data has changed since the previous copy has been overwritten. Instead, you need to create a new copy of `myData` and change only the parts of it that need to be changed. Then you can compare the old copy of `myData` with the new one in `shouldComponentUpdate()` using triple-equals:

```js
const newData = deepCopy(myData);
newData.x.y.z = 7;
newData.a.b.push(9);
```

Unfortunately, deep copies are expensive, and sometimes impossible.
You can alleviate this by only copying objects that need to be changed and by reusing the objects that haven't changed. Unfortunately, in today's JavaScript this can be cumbersome:

```js
const newData = Object.assign({}, myData, {
  x: Object.assign({}, myData.x, {
    y: Object.assign({}, myData.x.y, {z: 7}),
  }),
  a: Object.assign({}, myData.a, {b: myData.a.b.concat(9)})
});
```

While this is fairly performant (since it only makes a shallow copy of `log n`
objects and reuses the rest), it's a big pain to write. Look at all the
repetition! This is not only annoying, but also provides a large surface area
for bugs.

## [why not use DeepClone in React?](https://github.com/immerjs/immer/issues/619)

cloneDeep will always fully clone your entire state. This causes two
fundamental problems which makes it quite unusable to use with react for
any slightly significant amount of data:

1. deepClone is very expensive.

You have a collection of 100 items? All those items will continuously be retreated on every update. Hitting both the performance and the garbage collector badly

2. deepClone can't leverage memoization,

Because the "same" objects will not be refererrentially equal after every update. So it means that react will always need to rerender all your components.Which basically kills all benefits that immutability would offer you in the first place.

In other words, deepClone, unlike produce, doesn't offer structural sharing
of all the unchanged parts of your state after an update


## [immer vs deepClone](https://www.measurethat.net/Benchmarks/Show/10865/0/lodash-clonedeep-vs-immer-produce)
![d048b13e0dd5e84075e5b564d2aa6f37.png](/assets/d048b13e0dd5e84075e5b564d2aa6f37-xiyn63efe40a.png)

https://stackoverflow.com/questions/54999078/deepcopy-object-in-javascript-using-immer


## [structuredClone()](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone)

## Read more
- https://stackoverflow.com/questions/34385243/why-is-immutability-so-important-or-needed-in-javascript
- https://desalasworks.com/article/immutability-in-javascript-a-contrarian-view/
- https://kipalog.com/posts/Vi-sao-khong-nen-mutate-state-va-khong-nen-dung-cloneDeep-trong-react

- https://github.com/kolodny/immutability-helper

# Memoization

## In general

Memoization: a performance optimization technique which eliminates the need to recompute a value for a given input by storing the original computation and returning that stored value when the same input is provided.

Caching is a form of memoization.

Here's a simple implementation of memoization:

```ts
const values = {}
function addOne(num: number) {
  if (values[num] === undefined) {
    values[num] = num + 1 // <-- here's the computation
  }
  return values[num]
}
```

One other aspect of memoization is value referential equality. For example:

```ts
const dog1 = new Dog('sam')
const dog2 = new Dog('sam')
console.log(dog1 === dog2) // false
```

Even though those two dogs have the same name, they are not the same. However,
we can use memoization to get the same dog:

```ts
const dogs = {}
function getDog(name: string) {
  if (dogs[name] === undefined) {
    dogs[name] = new Dog(name)
  }
  return dogs[name]
}

const dog1 = getDog('sam')
const dog2 = getDog('sam')
console.log(dog1 === dog2) // true
```

You might have noticed that our memoization examples look very similar.
Memoization is something you can implement as a generic abstraction:

```ts
function memoize<ArgType, ReturnValue>(cb: (arg: ArgType) => ReturnValue) {
  const cache: Record<ArgType, ReturnValue> = {}
  return function memoized(arg: ArgType) {
    if (cache[arg] === undefined) {
      cache[arg] = cb(arg)
    }
    return cache[arg]
  }
}

const addOne = memoize((num: number) => num + 1)
const getDog = memoize((name: string) => new Dog(name))
```

Our abstraction only supports one argument, if you want to make it work for any
type/number of arguments, knock yourself out.

## Memoization in React

Luckily, in React we don't have to implement a memoization abstraction. They
made two for us! `useMemo` and `useCallback`.

For more on this read:
[Memoization and React](https://epicreact.dev/memoization-and-react).

You know the dependency list of `useEffect`? Here's a quick refresher:

```javascript
React.useEffect(() => {
  window.localStorage.setItem('count', count)
}, [count]) // <-- that's the dependency list
```

Remember that the dependency list is how React knows whether to call your
callback (and if you don't provide one then React will call your callback every
render). It does this to ensure that the side effect you're performing in the
callback doesn't get out of sync with the state of the application.

But what happens if I use a function in my callback?

```javascript
const updateLocalStorage = () => window.localStorage.setItem('count', count)
React.useEffect(() => {
  updateLocalStorage()
}, []) // <-- what goes in that dependency list?
```

We could just put the `count` in the dependency list and that would
actually/accidentally work, but what would happen if one day someone were to
change `updateLocalStorage`?

```diff
- const updateLocalStorage = () => window.localStorage.setItem('count', count)
+ const updateLocalStorage = () => window.localStorage.setItem(key, count)
```

Would we remember to update the dependency list to include the `key`? Hopefully
we would. But this can be a pain to keep track of dependencies. Especially if
the function that we're using in our `useEffect` callback is coming to us from
props (in the case of a custom component) or arguments (in the case of a custom
hook).

Instead, it would be much easier if we could just put the function itself in the
dependency list:

```javascript
const updateLocalStorage = () => window.localStorage.setItem('count', count)
React.useEffect(() => {
  updateLocalStorage()
}, [updateLocalStorage]) // <-- function as a dependency
```

The problem with that though it will trigger the `useEffect` to run every
render. This is because `updateLocalStorage` is defined inside the component
function body. So it's re-initialized every render. Which means it's brand new
every render. Which means it changes every render. Which means... you guessed
it, our `useEffect` callback will be called every render!

**This is the problem `useCallback` solves**. And here's how you solve it

```javascript
const updateLocalStorage = React.useCallback(
  () => window.localStorage.setItem('count', count),
  [count], // <-- yup! That's a dependency list!
)
React.useEffect(() => {
  updateLocalStorage()
}, [updateLocalStorage])
```

What that does is we pass React a function and React gives that same function
back to us... Sounds kinda useless right? Imagine:

```javascript
// this is not how React actually implements this function. We're just imagining!
function useCallback(callback) {
  return callback
}
```

Uhhh... But there's a catch! On subsequent renders, if the elements in the
dependency list are unchanged, instead of giving the same function back that we
give to it, React will give us the same function it gave us last time. So
imagine:

```javascript
// this is not how React actually implements this function. We're just imagining!
let lastCallback
function useCallback(callback, deps) {
  if (depsChanged(deps)) {
    lastCallback = callback
    return callback
  } else {
    return lastCallback
  }
}
```

So while we still create a new function every render (to pass to `useCallback`),
React only gives us the new one if the dependency list changes.

In this exercise, we're going to be using `useCallback`, but `useCallback` is
just a shortcut to using `useMemo` for functions:

```ts
// the useMemo version:
const updateLocalStorage = React.useMemo(
  // useCallback saves us from this annoying double-arrow function thing:
  () => () => window.localStorage.setItem('count', count),
  [count],
)

// the useCallback version
const updateLocalStorage = React.useCallback(
  () => window.localStorage.setItem('count', count),
  [count],
)
```

🦉 A common question with this is: "Why don't we just wrap every function in
`useCallback`?"

You can read about this in my blog post
[When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback).

🦉 And if the concept of a "closure" is new or confusing to you, then
[give this a read](https://mdn.io/closure). (Closures are one of the reasons
it's important to keep dependency lists correct.)

## Optimize React re-renders without memo
- https://overreacted.io/before-you-memo/
- https://kentcdodds.com/blog/optimize-react-re-renders

# [Fragment](https://reactjs.org/docs/fragments.html)
Fragments let you group a list of children without adding extra nodes to the DOM.

## Motivation
A common pattern is for a component to return a list of children.
```js
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}
```

`<Columns />` would need to return multiple `<td>` elements in order for the rendered HTML to be valid. If a parent div was used inside the `render()` of `<Columns />`, then the resulting HTML will be invalid.
```js
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```
results in a `<Table />` output of:
```js
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
```
Fragments solve this problem.

## Short Syntax
There is a new, shorter syntax you can use for declaring fragments. It looks like empty tags:
```js
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```
You can use `<></>` the same way you’d use any other element except that it doesn’t support keys or attributes.

## Keyed Fragments
Fragments declared with the explicit `<React.Fragment>` syntax may have keys.

A use case for this is mapping a collection to an array of fragments — for example, to create a description list:

```js
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // Without the `key`, React will fire a key warning
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```
key is the only attribute that can be passed to Fragment.


# [Error Boundaries](https://reactjs.org/docs/error-boundaries.html)

## what is Error Boundaries?

It's a special kind of component to handle errors in React application.

Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed.

Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

There is currently no way to create an Error Boundary component with a function and you have to use a class component instead.

## Some errors that Error boundaries do not catch:

- Event handlers (learn more)
- Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)
- Server side rendering
- Errors thrown in the error boundary itself (rather than its children)


# HOCs

https://medium.com/javascript-scene/do-react-hooks-replace-higher-order-components-hocs-7ae4a08b7b58
