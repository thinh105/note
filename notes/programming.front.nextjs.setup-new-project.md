---
id: y7bd3k9d2488gmf1ucowld7
title: Setup New Project
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Setup new project
updated_imported: '2022-02-26T17:52:54.000Z'
created_imported: '2022-02-26T08:02:04.000Z'
---

[TOC]











# Create new Next Project



# [Set up ESLint with NextJS](https://nextjs.org/docs/basic-features/eslint#eslint-plugin)


# Yarn - How do I update each dependency in package.json to the latest version?


```bash
yarn upgrade-interactive --latest
```


You can try this npm package yarn-upgrade-all.

This package will remove every package in package.json and add it again which will update it to latest version.

```bash

npx yarn-upgrade-all 

# Output
@thinh-pc ➜ tourino-next git:(main) ✗ npx yarn-upgrade-all
Need to install the following packages:
  yarn-upgrade-all
Ok to proceed? (y) y
 [Start]: yarn add next react react-dom 
 [Done]: yarn add next react react-dom 
 [Start]: yarn add --dev @types/node @types/react @typescript-eslint/eslint-plugin eslint eslint-config-airbnb-base eslint-config-next eslint-config-prettier prettier typescript 
 [Done]: yarn add --dev @types/node @types/react @typescript-eslint/eslint-plugin eslint eslint-config-airbnb-base eslint-config-next eslint-config-prettier prettier typescript 
@thinh-pc ➜ tourino-next git:(main) ✗ 
```

![7252c957a818b18a04b183e725ba9bc5.png](/assets/7252c957a818b18a04b183e725ba9bc5-uxbcq3ov0li3.png)

# Setup ES Lint packages

Have to visit npm page to see the correct way to install these packages.


## Install `eslint-config-airbnb`

```bash
npx install-peerdeps --dev eslint-config-airbnb
```

## Install eslint-config-airbnb-typescript

# [Start a clean Next.js project with TypeScript, ESLint and Prettier](https://paulintrognon.fr/blog/typescript-prettier-eslint-next-js)