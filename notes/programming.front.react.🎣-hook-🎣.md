---
id: l74lf7skac8h6ldiyqgjhk1
title: "\U0001F3A3 Hook \U0001F3A3"
desc: ''
updated: 1663409164564
created: 1663409164564
isDir: false
latitude: 20.8567
longitude: 106.6826
altitude: 0
title_imported: "\U0001F3A3 HOOK \U0001F3A3"
updated_imported: '2022-01-26T13:26:05.000Z'
created_imported: '2022-01-02T03:53:07.000Z'
---

[TOC]































* * *

# useLayoutEffect

## https://kentcdodds.com/blog/useeffect-vs-uselayouteffect
### Summary
1. `useLayoutEffect`:

If you need to mutate the DOM and/or do need to perform measurements

2. `useEffect`:

If you don't need to interact with the DOM at all or your DOM changes are unobservable (seriously, most of the time you should use this).

> Here’s the simple rule for when you should use useLayoutEffect: If you are making observable changes to the DOM, then it should happen in useLayoutEffect, otherwise useEffect.

# useContext

`Context` also has the unique ability to be scoped to a specific section of the React component tree. A common mistake of context (and generally any “application” state) is to make it globally available anywhere in your application when it’s actually only needed to be available in a part of the app (like a single page). Keeping a context value scoped to the area that needs it most has improved performance and maintainability characteristics.

# Rules of Hooks
1. Only call hooks at the Top Level

Don’t call Hooks inside loops, conditions, or nested functions.

Instead, always use Hooks at the top level of your React function, before any early returns

2. Only call hooks from React Functions

✅ Call Hooks from React function components.

✅ Call Hooks from custom Hooks

# useCallback

## `useCallback` use cases
The entire purpose of `useCallback` is to memoize a callback for use in dependency lists and props on memoized components (via` React.memo`).

The only time it’s useful to use `useCallback` is when the function you’re memoizing is used in one of those two situations:
1. Referential equality
2. Computationally expensive calculations

Read more: Why don’t we just wrap every function in useCallback? - [When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback).

## `useCallback` is just a shortcut to using useMemo for functions:

```js
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


# [When should I useMemo and useCallback?](https://kentcdodds.com/blog/usememo-and-usecallback)

There are specific reasons both of these hooks are built-into React:

1. Referential equality
2. Computationally expensive calculations



# useReducer
## Example

```js
  import * as React from 'react'

  const countReducer = (state, action) => {
    const { type, step } = action

    switch (type) {
      case 'INCREMENT': {
        return {
          ...state,
          count: state.count + step,
        }
      }
      default: {
        throw new Error(`Unhandled action type: ${type}`)
      }
    }
  }

  function Counter({ initialCount = 0, step = 1 }) {
    const [state, dispatch] = React.useReducer(countReducer, {
      count: initialCount,
    })

    const { count } = state

    const incrementByValue = () => dispatch({ type: 'INCREMENT', step })

    return <button onClick={incrementByValue}>{count}</button>
  }

  function App() {
    return <Counter />
  }

  export default App


```

## lazy initialization

This one's not an extra credit, but _sometimes_ lazy initialization can be
useful, so here's how we'd do that with our original hook App:

```javascript
function init(initialStateFromProps) {
  return {
    pokemon: null,
    loading: false,
    error: null,
  }
}

// ...

const [state, dispatch] = React.useReducer(reducer, props.initialState, init)
```

So, if you pass a third function argument to `useReducer`, it passes the second
argument to that function and uses the return value for the initial state.

This could be useful if our `init` function read into localStorage or something
else that we wouldn't want happening every re-render.

## The full `useReducer` API

If you're into TypeScript, here's some type definitions for `useReducer`:

> Thanks to [Trey's blog post](https://levelup.gitconnected.com/db1858d1fb9c)

> Please don't spend too much time reading through this by the way!

```typescript
type Dispatch<A> = (value: A) => void
type Reducer<S, A> = (prevState: S, action: A) => S
type ReducerState<R extends Reducer<any, any>> = R extends Reducer<infer S, any>
  ? S
  : never
type ReducerAction<R extends Reducer<any, any>> = R extends Reducer<
  any,
  infer A
>
  ? A
  : never

function useReducer<R extends Reducer<any, any>, I>(
  reducer: R,
  initializerArg: I & ReducerState<R>,
  initializer: (arg: I & ReducerState<R>) => ReducerState<R>,
): [ReducerState<R>, Dispatch<ReducerAction<R>>]

function useReducer<R extends Reducer<any, any>, I>(
  reducer: R,
  initializerArg: I,
  initializer: (arg: I) => ReducerState<R>,
): [ReducerState<R>, Dispatch<ReducerAction<R>>]

function useReducer<R extends Reducer<any, any>>(
  reducer: R,
  initialState: ReducerState<R>,
  initializer?: undefined,
): [ReducerState<R>, Dispatch<ReducerAction<R>>]
```

`useReducer` is pretty versatile. The key takeaway here is that while
conventions are useful, understanding the API and its capabilities is more
important.

## comparing  `useState`  and  `useReducer`:

### [Should I useState or useReducer?](https://kentcdodds.com/blog/should-i-usestate-or-usereducer)
Conclusion

So if you want some "rules" (NOT ESLINT RULES), here they are:

1. When it's just an independent element of state you're managing: useState
2. When one element of your state relies on the value of another element of your state in order to update: useReducer


### Read more [How to implement useState with useReducer](https://kentcdodds.com/blog/how-to-implement-usestate-with-usereducer)

# useRef

```js
const refContainer = useRef(initialValue);
```

## Definition
`useRef` returns a mutable ref object whose .current property is initialized to the passed argument (`initialValue`). The returned object will persist for the full lifetime of the component.

## Usecases

### Access a child imperatively to access the DOM
```js
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

### [Keeping any mutable value around (same instance property in class components)](https://reactjs.org/docs/hooks-faq.html#is-there-something-like-instance-variables)

The `useRef()` Hook isn’t just for DOM refs. The `“ref”` object is a generic container whose current property is mutable and can hold any value, similar to an instance property on a class.

```js
function Timer() {
  const intervalRef = useRef();

  useEffect(() => {
    const id = setInterval(() => {
      // ...
    });
    intervalRef.current = id;
    return () => {
      clearInterval(intervalRef.current);
    };
  });

  // ...
}
```

https://reactjs.org/docs/hooks-reference.html#useref

https://react-hooks.netlify.app/5

https://dmitripavlutin.com/react-useref-guide/

# useState

## Functional updates

If the new state is computed using the previous state, you can pass a function to setState.

The function will receive the previous value, and return an updated value. Here’s an example of a counter component that uses both forms of setState:

```js
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```

The ”+” and ”-” buttons use the functional form, because the updated value is based on the previous value. But the “Reset” button uses the normal form, because it always sets the count back to the initial value.

If your update function returns the exact same value as the current state, the subsequent rerender will be skipped completely.

### [If not using **functional update**, you may get an unexpected error.](https://stackoverflow.com/questions/55342406/updating-and-merging-state-object-using-react-usestate-hook)

You need to be careful when updating state derived from something that already is in state.

If you e.g. update a count twice in a row, it will not work as expected if you don't use the function version of updating the state.

```js
const { useState } = React;

function App() {
  const [count, setCount] = useState(0);

  function brokenIncrement() {
    setCount(count + 1);
    setCount(count + 1);
  }

  function increment() {
    setCount(count => count + 1);
    setCount(count => count + 1);
  }

  return (
    <div>
      <div>{count}</div>
      <button onClick={brokenIncrement}>Broken increment</button>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

Read more: [useState lazy initialization and function updates](https://kentcdodds.com/blog/use-state-lazy-initialization-and-function-updates)



### Eslint also complains if you forget to use functional update

> React Hook useEffect has a missing dependency: 'state'. Either include it or remove the dependency array. You can also do a functional update 'setState(s => ...)' if you only need 'state' in the 'setState' call. eslintreact-hooks/exhaustive-deps

![0a43c4cedae483946caac329dcb7e6b7.png](/assets/0a43c4cedae483946caac329dcb7e6b7-zbk0n42rtr93.png)

## useState is not merge update objects

In useState hooks, when we update a state variable, **we replace its value**. 

This is different from `this.setState` in a class, which **merges the updated fields** into the object.

You can replicate this behavior by combining the function updater form with object spread syntax:

```js
const [state, setState] = useState({});
setState(prevState => {
  // Object.assign would also work
  return {...prevState, ...updatedValues};
});

```

However, we React teams **recommend to split state into multiple state variables based on which values tend to change together.**

For Nested states, consider using [immerjs](https://github.com/immerjs/immer)

Another option is `useReducer`, which is more suited for **managing state objects that contain multiple sub-values**.

Read more:

- https://reactjs.org/docs/hooks-faq.html#should-i-use-one-or-many-state-variables

## Dont Forget to push the state back down

- We’re pretty good at lifting state. But We usually forget to push the state back down (or [colocate state](https://kentcdodds.com/blog/state-colocation-will-make-your-react-app-faster)).
    
- [Colocation](https://kentcdodds.com/blog/colocation) : Đặt các file/logic/comments/... liên quan ở cùng nhau để tiện làm việc
    

Source: EpicReact

## Derived state

- If some states are calculated by another related state, we can turn them into derived states.
    
- Now we can pass around and use them, and they will automatically update whenever the state that they depend on, changes.
    
- If the performance is concern, we can use useMemo,
    
- Further Reading
    
    - [Don’t Sync State. Derive It!](https://kentcdodds.com/blog/dont-sync-state-derive-it)

Source: EpicReact

- **My though:** Derived State + useMemo is kind of the same with Computed property in Vue

## [lazy state initialization](https://kentcdodds.com/blog/use-state-lazy-initialization-and-function-updates)
Sometimes, we have some expensive Computation (get from local storage, from the API, etc) to get the value for the useState hook.

This is problematic because it could be a performance bottleneck. And what’s more we only actually need to know the value from that expensive computation the first time this component is rendered!

So the additional reads are wasted effort.

To avoid this problem, React’s `useState` hook allows you to pass a function instead of the actual value, and then it will only call that function to get the state value when the component is rendered the first time.


```js
//So you can go from this:
React.useState(someExpensiveComputation())

//to this:
React.useState(() => someExpensiveComputation())
```
And the someExpensiveComputation function will only be called when it’s needed!

## using a shallow comparison of the dependency values
- the return function will run when component unmount
- the dependency values changes  
- https://overreacted.io/a-complete-guide-to-useeffect/

## [lazy state initialization](https://kentcdodds.com/blog/use-state-lazy-initialization-and-function-updates)

React’s useState hook allows you to pass a function instead of the actual value, and then it will only call that function to get the state value when the component is rendered the first time. So you can go from this:

`React.useState(someExpensiveComputation())`

To this:

`React.useState(() => someExpensiveComputation())`

And the `someExpensiveComputation` function will only be called when it’s needed!


# useEffect

## Empty Dependency Array

- From the react docs:
    
    > Passing in an empty array \[\] of inputs tells React that your effect doesn’t depend on any values from the component, so that effect would run only on mount and clean up on unmount; it won’t run on updates

## [Cannot using async await directly in useEffect](https://react-hooks.netlify.app/6)

```js
    // this does not work, don't do this:
    React.useEffect(async () => {
      const result = await doSomeAsyncThing()
      // do something with the result
    })
```

- Why?
    
    - the useEffect hook is that you cannot return anything other than the cleanup function.
        
    - But when you make a function async, it automatically returns a promise (whether you’re not returning anything at all, or explicitly returning a function).
        
    - This is due to the semantics of async/await syntax.
        
- How to solve that problem?
    

```js
React.useEffect(() => {
  async function effect() {
    const result = await doSomeAsyncThing()
    // do something with the result
  }
  effect()
})
```

This ensures that you don’t return anything but a cleanup function.

🦉 I find that it’s typically just easier to extract all the async code into a utility function which I call and then use the promise-based .then method instead of using async/await syntax:

```js
React.useEffect(() => {
doSomeAsyncThing().then(result => {
// do something with the result
})
})
```

Source: EpicReact

## Futher reading:

- https://www.robinwieruch.de/react-hooks-fetch-data/
- https://www.robinwieruch.de/react-useeffect-hook/
- 