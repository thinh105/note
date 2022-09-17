---
id: 81ueh9rlvxnvfjld6g7zk8q
title: Tailwindcss
desc: ''
updated: 1663409164561
created: 1663409164561
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: TailwindCSS
updated_imported: '2022-04-10T16:39:51.000Z'
created_imported: '2022-04-08T09:34:23.000Z'
---

# style a href link
https://stackoverflow.com/questions/63883580/tailwind-css-how-to-style-a-href-links-in-react

As Luke correctly explained, Tailwind's Preflight removes all the browser defaults. You'll have to add the styling yourself:
```
className="underline text-blue-600 hover:text-blue-800 visited:text-purple-600"
```
However, you can't just use visited: with text-purple-600 without some configuration (at least with Tailwind 2. I'm not familiar with older versions). You'll also need to add the following to your Tailwind config at your project root:
```
// tailwind.config.js
module.exports = {
  // ...
  variants: {
    extend: {
      textColor: ['visited'],
    }
  },
}
```
That way, Tailwind will make all the classes with visited: available for use with all the text color classes.

You can learn more about enabling extra variants in the Tailwind docs.