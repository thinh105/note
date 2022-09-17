---
id: 2vop974em1yk4pylb1nr79k
title: Custom Hooks
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Custom Hooks
updated_imported: '2022-01-08T11:55:29.000Z'
created_imported: '2022-01-08T11:37:38.000Z'
---

# List of custom Hooks

- [Usehooks](https://usehooks.com/)

This website provides easy to understand code examples to help you learn how hooks work and inspire you to take advantage of them in your next project. 

https://github.com/gragland/usehooks

- [Awesome React Hooks Resources](https://github.com/rehooks/awesome-react-hooks)

- [React Recipes](https://github.com/craig1123/react-recipes)

A list of React Hooks utility library containing popular customized hooks




# [React Hooks Form](https://www.npmjs.com/package/react-hook-form)


# [React Query](https://react-query.tanstack.com/)

> Really, I'd just use react-query personally, but bear with me for the purpose of the example...
> [Kent C. Dodds @Epic React](https://epicreact.dev/memoization-and-react/)

# [React Error Boundary](https://github.com/bvaughn/react-error-boundary).

> Kent C. Dodds @ EpicReact : As cool as our own ErrorBoundary is, I’d rather not have to maintain it in the long-term. Luckily for us, there’s an npm package we can use instead and it’s already installed into this project. It’s called [react-error-boundary](https://github.com/bvaughn/react-error-boundary).

**About this package:** This component provides a simple and reusable wrapper that you can use to wrap around your components. Any rendering errors in your components hierarchy can then be gracefully handled.

Reading this blog post will help you understand what react-error-boundary does for you: [Use react-error-boundary to handle errors in React](https://kentcdodds.com/blog/use-react-error-boundary-to-handle-errors-in-react) – How to simplify your React apps by handling React errors effectively with react-error-boundary

### reset ErrorBoundary State

Luckily for us `react-error-boundary` supports the way to reset itself automatically with the `resetKeys` prop.

You pass an array of values to resetKeys and if the ErrorBoundary is in an error state and any of those values change, it will reset the error boundary.