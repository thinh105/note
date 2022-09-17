---
id: 5sx08vfcc3ok2ae8sbmnqsz
title: Nodejs
desc: ''
updated: 1663409164558
created: 1663409164558
isDir: false
title_imported: Nodejs
updated_imported: '2022-01-28T16:02:57.000Z'
created_imported: '2019-10-30T17:13:26.000Z'
---

[TOC]
















# NodeJS best practice
https://github.com/goldbergyoni/nodebestpractices



# Error in NPM when update new Node version

Update new npm to fix that

```sh
npm install npm@latest -g
```

# exports return a function, not a value - not set equal to array ( reference Type )

`module.exports.e = e;` copies the value of `e` at the time the statement is evaluated, so even if you did change `e`, it wouldn't change the value of the exported property [@source](https://stackoverflow.com/questions/42352348/using-exports-in-nodejs-to-return-a-value)

*exportFunc.js*
```js
let todos = [];

exports.pushArray = () => {
  todos.push('push');
  return todos; // [ 'push' ]
};

exports.reassignArray = () => {
  todos = ['reassign', 'Array'];
  return todos; // [ 'reassign', 'Array' ]
};

//DON'T

exports.todos = todos; // [ 'push' ]

//DO
exports.todos = () => {
  return todos; // [ 'reassign', 'Array' ]
};
```

*test.js*
```js
const Todo = require('./exportFunc');

// DON'T
// console.log(Todo.pushArray(), Todo.reassignArray(), Todo.todos);
// [ 'push' ] [ 'reassign', 'Array' ] [ 'push' ]

//DO
console.log(Todo.pushArray(), Todo.reassignArray(), Todo.todos());

// [ 'push' ] [ 'reassign', 'Array' ] [ 'reassign', 'Array' ]


```


*or simple version*
```js
let todo = [];

todo.push(123); // [ 123 ]

//Don't
const a = todo; // a = [ 123 ]

todo.push(567); // todo = [ 123, 567 ] ; a = [ 123, 567 ]

todo = [456]; // a = [ 123, 567 ]

console.log(todo, a); // [ 456 ] [ 123, 567 ]
```


# Initiation a NodeJS project

```bash

mkdir [Project-name]

cd [Project-name]

git init

npm init -y

cp /home/myProjects/Node/"Jonas Course - Natour"/{.prettierrc,.gitignore,.eslintrc.json} /home/myProjects/Node/learn-mongoose 

npm i
#...
```

## Specify the Node version to avoid ESlint Error for using new feature
*in package,json*
```js
"engines" : {
"node": ">=10.0.0"
}
```

## Using __dirname in fs module

```js
const  fs  =  require('fs');
const tours = JSON.parse(
  fs.readFileSync(`${__dirname}/tours-simple.json`, 'utf8')
);
//using __dirname instead of ./
```



# Error: listen EADDRINUSE: address already in use :::6969 - Kill Nodemon when it meet error and cannot kill normally
*in termial*
```bash
kill -9 $(lsof -t -i:6969) #6969 is the port
```

# Resolving EACCES permissions errors when installing NPM packages globally
To minimize the chance of permissions errors, you can configure npm to use a different directory. In this example, you will create and use hidden directory in your home directory.

Back up your computer.
On the command line, in your home directory, create a directory for global installations:
```bash
 mkdir ~/.npm-global
```
Configure npm to use the new directory path:
```bash
 npm config set prefix '~/.npm-global'
```
In your preferred text editor, open or create a ~/.profile file and add this line:
```bash
 export PATH=~/.npm-global/bin:$PATH
```
On the command line, update your system variables:
```bash
source ~/.profile
```
To test your new configuration, install a package globally without using sudo:
```bash
 npm install -g jshint
```
[@source](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)

#  Install Eslint on VScode
1. Install th
2. 
```bash
npm i eslint prettier  
```c

easdcwq
# Best practices
- [Node.JS best Practise](https://github.com/goldbergyoni/nodebestpractices)

# Error handling with async-await

https://dev.to/sobiodarlington/better-error-handling-with-async-await-2e5m

https://www.linkedin.com/pulse/promise-promises-david-mark/

https://itnext.io/error-HANDLING-WITH-async-await-in-js-26c3f20bc06a

https://medium.com/@jesterxl/easier-error-handling-using-async-await-b9ab0cb938e

https://nec.is/writing/the-ultimate-guide-for-error-handling-with-async-await/


https://codeburst.io/node-express-async-code-and-error-handling-121b1f0e44ba

# Capture error and data in async-await without try-catch

https://blog.grossman.io/how-to-write-async-await-without-try-catch-blocks-in-javascript/ => https://github.com/scopsy/await-to-js

https://dev.to/sadarshannaiynar/capture-error-and-data-in-async-await-without-try-catch-1no2
Csde .
https://itnext.io/async-await-without-try-catch-in-javascript-6dcdf705f8b1
