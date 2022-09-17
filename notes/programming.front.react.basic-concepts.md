---
id: n99ziy13c3wed2qdmcgt3c1
title: Basic Concepts
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
title_imported: Basic Concepts
updated_imported: '2022-01-18T15:36:14.000Z'
created_imported: '2021-02-01T02:40:21.000Z'
---


# Key in React

## Rules of keys
- Keys must be unique among siblings. However, it’s okay to use the same keys for JSX nodes in different arrays.

- Keys must not change or that defeats their purpose! Don’t generate them while rendering.
	Do not generate keys on the fly, e.g. with `key={Math.random()}`.
	
	This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time.
	
	Not only is this slow, but it will also lose any user input inside the list items.
	
	Instead, **use a stable ID based on the data.**
	
> Note that your components won’t receive key as a prop. It’s only used as a hint by React itself. If your component needs an ID, you have to pass it as a separate prop: <Profile key={id} userId={id} />.


https://kentcdodds.com/blog/understanding-reacts-key-prop
