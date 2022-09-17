---
id: 4nk7y9a7xs7z0bhcft5hll6
title: "\U0001F383 Redux \U0001F383"
desc: ''
updated: 1663409164564
created: 1663409164564
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: "\U0001F383 Redux \U0001F383"
updated_imported: '2022-02-08T08:11:08.000Z'
created_imported: '2022-01-28T11:33:31.000Z'
---

[TOC]
























# Redux is a predictable state container for JavaScript apps.

Basically, Reducers pick up the information from Actions and “reduce” it to a new state that is saved in the Store.
When state in the Store is changed, the View can act on this by subscribing to the Store.

```js
View -> Action -> Reducer(s) -> Store -> View
```

# Actions

An action in Redux is a JavaScript object. It has a type and an optional payload

```js
{
type: 'TODO_ADD',
todo: { id: '0', name: 'learn redux', completed: false },
}
```

Executing an action is called to dispatch in Redux. You can dispatch an action to alter the state in the Redux store.
You only dispatch when you want to change the state. The dispatch of an action can be triggered in your view layer.

# Reducer(s)
A reducer is the next part in the chain of the unidirectional data flow.
The view **dispatches** an action and the action object, with **action type** and **optional payload**, will pass through all reducers.

**A reducer is a pure function.** It always produces the same output when the input stays the same. It has no side-effects, thus it is only an input/output operation.

A reducer has two inputs: state and action.
1. The state is always the whole state object from the Redux store.
2. The action is the dispatched action with type and payload.

The reducer reduces (that explains the naming) the previous state and incoming action to a new state.

A reducer is a pure function without side-effects, it also embraces immutable data structures.

It always returns a newState object without mutating the incoming state object.

```js
// not allowed a reducer function
function(state, action) {
return state.push(action.todo);
}

// is allowed
function reducer(state, action) {
return state.concat(action.todo);
}
```


# Store

The Redux store holds one global state object.

It is a singleton.

There are no multiple stores and no multiple states.

```js
// Create Store

import { createStore } from 'redux';

const store = createStore(reducer)

// createStore takes a second optinal argument: the initial state.
const store = create(reducer, [])
```


# Dispatch an action

```js
store.dispatch({
	type: 'TODO_ADD',
	todo: {
		id: 0,
		name: 'learn redux',
		completed: false,
	}
})
```

# Get global state from the store

```js
store.getState()
```

# Subscribe/Unsubscribe to the store to lisen for updates
```js
const unsubscribe = store.subscribe(()=> {
	console.log(store.getState());
})

//don't forget to unsubscribe eventually
unsubscribe();
```


# Best practises: Advanced Actions

## Keep Minimum Action Payload

Only defined the necessary payload in the action.

## Should extract Action Types as variable

To make the application more robust, you should extract the action type as a variable.

Otherwise, you can run into typos where an action never reaches a reducer because you misspelled it.

```js
// Should extract Action Types as variable
const TODO_ADD = 'TODO_ADD';
const TODO_TOGGLE = 'TODO_TOGGLE';


const action = {
type: TODO_ADD,
todo: { id: '0', name: 'learn redux' },
};

const toggleTodoAction = {
type: TODO_TOGGLE,
todo: { id: '0' },
};


function reducer(state, action) {
	switch(action.type) {
		case TODO_ADD : {
			return applyAddTodo(state, action);
		}
		case TODO_TOGGLE : {
			return applyToggleTodo(state, action);
		}
		default : return state;
	}
}

```


We can define them in separate files. We would only need to import the action type to use them only in specific actions and reducers.

## Action Creator
We can add another layer on top, it's not mandatory, but they are convenient to use.

Action creators encapsulate the action with its action type and optional payload in a reusable function.

In addition, they give you the flexibility to pass any payload.

After all, they are only pure functions which return an object.


```js
//normal use
const TODO_ADD = 'TODO_ADD';

store.dispatch({
	type: TODO_ADD,
	todo: { id: '0', name: 'learn redux' },
});


//with Action Creator
function doAddTodo(id, name) {
	return {
		type: TODO_ADD,
		todo: { id, name },
	};
}

store.dispatch(doAddTodo('0', 'learn redux'));
```


## Optional Payload

Actions don't need to have a payload.
Only the action type is required.

```js

// action to open login modal

{
	type: 'LOGIN_MODAL_OPEN',
}

{
	type: 'LOGIN_MODAL_CLOSE',
}

```

That's verbosem because you need 2 actions to alter only one property in the state.

 If you used the optional payload for the action, you could solve login scenario in only one action instead of two actions. 

The `isLoginModalOpen` property would be dynamically passed in the action rather than being hardcoded in a reducer.

```js
{
	type: 'LOGIN_MODAL_TOGGLE',
	isLoginModalOpen: true,
}

// By using an action creator, the payload can be passed in as arguments and thus stays flexible.

function doToggleLoginModal(open) {
	return {
		type: 'LOGIN_MODAL_TOGGLE',
		isLoginModalOpen: open,
	};
}

```

## Payload Structure

```js
// instead of

{
	type: 'TODO_ADD_ASSIGNED',
	todo: { id: '0', name: 'learn redux' },
	assignedTo: { id: '99' name: 'Robin' },
}

// let do
{
	type: 'TODO_ADD_ASSIGNED',
	payload: {
		todo: { id: '0', name: 'learn redux' },
		assignedTo: { id: '99' name: 'Robin' },
	},
}
```

The refactoring ensures that type and payload are visible on first glance.

As said, it is not mandatory to do so and often adds more complexity.

But in larger applications it can keep your action creators readable.


# Advanced Reducers

This is best practises for reducers, not everything is mandatory, but you should at least know about the following things to embrace best practices, common usage patterns and practices on scaling your state architecture

## Initial State

```js

// normal use
const store = createStore(reducer, []);


// default initial state

const store = createStore(reducer);

function reducer(state = [], action) {
	switch(action.type) {
		case TODO_ADD : {
			return applyAddTodo(state, action);
		}
		case TODO_TOGGLE : {
			return applyToggleTodo(state, action);
		}
		default : return state;
	}
}
```


## Avoid Nested Data Structures

Nested data structures are fine in Redux, but you want to avoid deeply nested data structures.

We can see, it adds complexity to create your new state object. 


## Combined Reducer

Redux gives you a helper to combine multiple reducers into one root reducer: `combineReducers().`

The function takes an object as input that has a state as property name and a reducer as value.

```js
const rootReducer = combineReducers({
	todoState: todoReducer,
	filterState: filterReducer,
});


// Afterward, the rootReducer can be used to initialize the Redux store instead of the single reducer.

const store = createStore(rootReducer);

function todoReducer(state = [], action) {
	switch(action.type) {
		case TODO_ADD : {
			return applyAddTodo(state, action);
		}
		case TODO_TOGGLE : {
			return applyToggleTodo(state, action);
		}
		default : return state;
	}
}

function filterReducer(state = 'SHOW_ALL', action) {
	switch(action.type) {
		case FILTER_SET : {
			return applySetFilter(state, action);
		}
		default : return state;
	}
}

// global sate:
{
	todoState: ...,
	filterState: ...,
}

// In each reducer, the incoming state is not the global state object anymore.
// It is their defined substate from the `combinedReducers()` function.

// The `todoReducer` doesn’t know anything about the `filterState` and the `filterReducer` doesn’t know anything about the `todoState`.


function applyAddTodo(state, action) {
	const todo = Object.assign({}, action.todo, { completed: false });
	return state.concat(todo);
}

function applyToggleTodo(state, action) {
	return state.map(todo =>
		todo.id === action.todo.id
		? Object.assign({}, todo, { completed: !todo.completed })
		: todo
		);
}

function applySetFilter(state, action) {
	return action.filter;
}


```


# Clarification for Initial State

If we want to use initial state in the reducer, we have to remove the intial state in `createStore()`

- One plain reducer: When using only one plain reducer, the initial state in `createStore()` dominates the initial state in the reducer. The initial state in the reducer only works when the incoming initial state is undefined because then it can apply a default state. But the initial state is already defined in `createStore()` and thus utilized by the reducer.

- Combined reducers: When using combined reducers, you can embrace a more nuanced usage of the state initialization. The initial state object that is used for the `createStore()` function doesn’t have to include all substates that are introduced by the combineReducers() function. Thus, when a substate is undefined, the reducer can define the default substate. Otherwise, the default substate from the `createStore()` is used.


## Nested Reducers

# Immutable State

Redux embraces an immutable state.

Your reducers will always return a new state object. You will never mutate the incoming state.

Therefore, you might have to get used to different JavaScript functionalities and syntax to embrace immutable data structures.

## Build-in JS functions

- array.map()
- array.concat(item)
- {...object}

## 3rd party library

- immutable.js
- ...

Personally I would recommend to use such libraries only in two scenarios:
• You are not comfortable to keep your data structures immutable with JavaScript ES5, JavaScript ES6 and beyond.
• You want to improve the performance of immutable data structures when using huge amounts of data.


JavaScript gives you enough tools to keep your data structures immutable.
There is no need to use a third-party library except for the two mentioned use cases. However, there might be a third use case where such library would help: deeply nested data structures in Redux that need to be kept immutable.

It is bad practice to have deeply nested data structures in Redux in the first place. We should keep your data structures flat.

# Normalized State

A best practice in Redux is a flat state structure

https://redux.js.org/tutorials/essentials/part-6-performance-normalization#normalizing-data

# Selectors
A selector is a function that takes the state as argument and returns a substate or derived properties of it

```js
(state) => derived properties
(state, arg) => derived properties
```

Instead of retrieving the state explicitly:
```js
function mapStateToProps(state) {
	return {
		todos: state.todoState,
	};
}
```

You would retrieve it implicity via a selector:
```js
function getTodos(state) {
	return state.todoState;
}
function mapStateToProps(state) {
	return {
		todos: getTodos(state),
	};
}
```
