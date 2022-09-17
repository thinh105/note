---
id: 18gv4fam3x8ws2dmdydd79h
title: 06 Environment Variable
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
title_imported: 06 Environment Variable
updated_imported: '2019-11-07T11:21:30.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---


## Section 6 Video 21 : Environment Variable
Node.JS and Express app can run in diffenrent enviroments.
Two most important are:
- development environment
- production enviroment

### Why need that?

- We use EV for configuration setting for our app.
- Different database for development and for testing ...
- turn login on or off
- turn debugging on or off
- store password username

Show Express environment we are running:
```js
console.log(app.get('env');
```
Show Node.js Environment:
```js
console.log(process.env)
```
 ####  Create configuration file config.env
 
```env
NODE_ENV = development

PORT =  6969

USERNAME = thinhnv

PASSWORD = 3liyIasdGyugGGy
```
#### Connect to the Node.js

terminal
``` sh
npm i dotenv
```

Node file - server.js
```js
const dotenv = require('dotenv');
dotenv.config({path: './config.env'});
```
#### Usage
In server.js file
```js
const port = process.env.PORT || 6969
// if process.env.PORT is false it will get the default 6969
```

In any files - ex: app.js
```js
//Make middleware only run when in development environment

if(process.env.NODE_ENV === 'development') {
console.log(' ... ... ');
app.use(morgan('dev')); 
//etc

}
```
The environment variables are on the process and it is available for every single file in the project - no need require anything

Remember to define dotenv before require any file that we want to get enviroment variables

Working:
```js
const dotenv = require('dotenv');
dotenv.config({path: './config.env'});

const app = require('./app');
```
Not Working
```js
const dotenv = require('dotenv');
const app = require('./app');

dotenv.config({path: './config.env'}); 
```

 #### Add start script to package.json

```js
"scripts" : {
	"start": "nodemon server.js",
	"start:prod": "NODE_ENV=production nodemon server.js"
	},
```

in terminal
```sh
npm run start:prod
```





<!--stackedit_data:
eyJoaXN0b3J5IjpbMzA2ODQ4MTcxLC0xMTg3Mjc4MzA3LC0xND
QzNTE5MzAzLC0xNjkyODEzODYzLC0xNTE0MTExOTEzXX0=
-->