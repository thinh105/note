---
id: s9rxr799dcqgsbhd2xn1xq6
title: 09 Error Handling with Express
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
title_imported: 09 Error Handling with Express
updated_imported: '2019-11-12T02:24:07.000Z'
created_imported: '2019-10-30T16:05:54.000Z'
---

[TOC]

# 2 Debugging Node
## NDB - node,js debugger by Google

*install*
```bash
sudo npm install -g ndb --unsafe-perm=true --allow-root
```

https://developers.google.com/web/tools/chrome-devtools/javascript/reference

# 3 Handling Unhandled Routes

A Route handler for a route that was not catched by any route handlers.

In Express, all the middleware functions are executed in the order they are in code 

So we will write this route in the last, so, it will catch the error URL

*In app.js*
```js
app.all('*', (req, res) => {
  res.status(404).json({
    status: 'fail',
    message: `Can't find '${req.originalUrl}' on this server!`
  });
});
```

# 4 An Overview of Error Handling
![enter image description here](http://i.imgur.com/TcSpyeE.png)

# 5 Write an Global Error Handling Middleware

*define an error handling middleware in app,js*
to define an error handling middleware function, we use 4 arguments in that  function
```js
app.use((err,req,res,next) =>{
	err.statusCode = err.statusCode || 500; // default is 500 - internal server error
	err.status = err.status || 'error';
	
	res.status(err.statusCode).json({
	status: err.status,
	message: err.message
	});
});
```

*Throw Error to the error handling middleware*

```js
app.all('*', (req, res, next) => {
  const err = new Error(`Can't find '${req.originalUrl}' on this server!`); // ( = error.message)
  err.status = 'fail';
  err.statusCode = 404;

  next(err); // skip all other middleware and send directly to the our global error handling middleware
});
```

# 6 Better Errors and Refactoring

Create `appError.js` in `utils` folder
Make a Class AppError extends from Error
<br>
```js
class AppError extends Error {
	constructor(message, statusCode) {
		super(message);
		
		this.statusCode = statusCode;
		this.status = `${statusCode}`.startWith('4') ? 'fail' : 'error';
		this.isOperational = true;

		Error.captureStackTrace(this, this.contructor);
		
	}
}

module.exports = AppError;
```

*stackTrace*

Can log it by the code to show us where the error happend :
```js
console.log(err.stack);
```

*app.js*
```js
const AppError = require( ... );

app.all('*',(req,res,next)=>{
	next(new AppError(`Can't find "${req.originalUrl}" on this server!`,404));
});
```

*Create `errorController.js` in `controllers` folder*

```js
module.exports = (err,req,res,next) =>{
	err.statusCode = err.statusCode || 500; // default is 500 - internal server error
	err.status = err.status || 'error';
	
	res.status(err.statusCode).json({
	status: err.status,
	message: err.message
	});
};
```

*in app.js*
```js
const globalErrorHandler = require('./controller/errorController');

app.use(globalErrorHandler);
```

# 7 Catching Errors in Async Functions

In all our Async Function (handle function: `getTour`, `getAllTour`, etc), we have `try catch` block.

```js

exports.createTour = async (req, res) => {
  try {
    const newTour = await Tour.create(req.body);

    res.status(201).json({
      status: 'success',
      data: { tour: newTour }
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: err
    });
  }
};

```

That will make our code look messy and unfocused, also we have a lot of duplicated code for `catch (err)`.

The solution is take the `try catch` block out of there and put it on a higher level.

We will create an asynchronous function `catchAsync` and will return promise.

when there is an error inside of an async function, that means the promise gets rejected.  

*tourController.js*

```js

const catchAsync = fn => {
    return(req,res,next) => {
        fn(req,res,next).catch(next);
        // catch(next) === catch( err => next(err) )
    };
};

exports.createTour = catchAsync( async (req, res,next ) => {
    const newTour = await Tour.create(req.body);
    res.status(201).json({
      status: 'success',
      data: { tour: newTour }
    });
});

```

Refactor to /util/catchAsync



### Test

https://repl.it/@ThinhNguyen19/Better-error-handling-with-asyncawait

# 8 Adding 404 Not Found Errors
When we get tour with ramdom ID, we will get this:

```json
{
  "status": "error",
  "message": "Cast to ObjectId failed for value \"5d99ad17cbc74c33e46d9c54s\" at path \"_id\" for model \"Tour\""
}
```

When we get tour with valid Mongo ID, we will get this:

```
{
  "status": "success",
  "data": {
    "tour": null
  }
}
```

So we need throw an error with a 404 status code when tour = null

**in tourController**
```
const AppError = require('../utils/appError');

exports.getTour = ... {

const tour = ...

if (!tour) return next(new AppError('No tour found with that ID!!!', 404));
// return to stop this function on that error
// if not, it will still response normal

res.status(200).json({ ...
    
}
```

# 9 Errors During Development vs Production
We need to send different error messages for the development and production environment.

In production, we want to leak as little information about our errors to the client as possible.

We only want to send a nice, human-freindly message so that the user knows what's wrong.

When in development, we want to get as much information about the error as possible


**in errorController.js file**
```js
const sendErrorDevelopment = (err, res) => {
  res.status(err.statusCode).json({
    status: err.status,
    error: err,
    message: err.message,
    stack: err.stack
  });
};

const sendErrorProduction = (err, res) => {
  res.status(err.statusCode).json({
    status: err.status,
    message: err.message
  });
};

module.exports = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500; // default is 500 - internal server error
  err.status = err.status || 'error';

  if (process.env.NODE_ENV === 'development') {
    sendErrorDevelopment(err, res);
  } else if (process.env.NODE_ENV === 'production') {
    sendErrorProduction(err, res);
  }
};
```
## About Operational Errors

**in appError.js file**
```js
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);

    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';

    this.isOperational = true;  

    Error.captureStackTrace(this, this.contructor);
  }
}

module.exports = AppError;

```

`this.isOperational = true;` means all the errors that we create ourselves will be operational errors.

In production, only these Operational error, we will send the error message down to the client.

When we have a programming error, or some other unknown error that comes from the third-party package, we must not send any error message about that to the client.


**in errorController.js file**
```js
const sendErrorProduction = (err, res) => {
  // Operational, trusted error: send message to client
  if (err.isOperational) {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.message
    });

    //Programming or other unknown error: don't leak error details
  } else {
    // 1) log error
    console.error('ERROR @#$%^', err);

    // 2) Send generic message to client
    res.status(500).json({
      status: 'error',
      message: 'Sorry, something went wrong!!!'
    });
  }
};

```
# Mongoose Error 

## 10 Handling Invalid Database IDs

identify this error by `err.name === 'CastError'`

**in errorController file**

```js
const AppError = require('../utils/appError');

const handleCastErrorDB = err => {
    const message = `Invalid ${err.path}: ${err.value}!!!`;
    return new AppError(message, 400);
}

...

module.exports = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500; // default is 500 - internal server error
  err.status = err.status || 'error';

  if (process.env.NODE_ENV === 'development') {
    sendErrorDevelopment(err, res);
  } else if (process.env.NODE_ENV === 'production') {
  
  // not a good practise to override the arguments of function.
  
  //create a hard copy
  
   let errorCopy = {...err} ;
   if (errorCopy.name === 'CastError') errorCopy = handleCastErrorDB(errorCopy);
   
   sendErrorProduction(errorCopy, res);
  }
}; 
```

## 11 Handling Duplicate Database Fields

identify this error by `error.code === 11000`
`errmsg: 'E11000 duplicate key error collection: natour.tours index: name_1 dup key: { : "Ha Nam Bay" }`

**in errorController file**
```js
const handleDuplicateFieldsDB = err => {
  // errmsg: 'E11000 duplicate key error collection: natour.tours index: name_1 dup key: { : "Ha Nam Bay" }',
  // Use Regex to get the value in errmsg

  const value = err.errmsg.match(/(["'])(\\?.)*?\1/)[0];
  console.log(value);

  const message = `Duplicate field value: ${value}. Please use another value!!!`;
  return new AppError(message, 400);
};

...

if (error.code === 11000) error = handleDuplicateFieldsDB(error);

```

## 12 Handling Mongoose Validation Errors


Error we get when PATCH invalid data

```json
"error": {
    "errors": {
      "ratingsAverage": {
        "message": "Rating must be below 5.0",
        "name": "ValidatorError",
        "properties": {
          "message": "Rating must be below 5.0",
          "type": "max",
          "max": 5,
          "path": "ratingsAverage",
          "value": 7
        },
        "kind": "max",
        "path": "ratingsAverage",
        "value": 7
      },
      "difficulty": {
        "message": "Difficulty is either: easy, medium, difficult",
        "name": "ValidatorError",
        "properties": {
          "message": "Difficulty is either: easy, medium, difficult",
          "type": "enum",
          "enumValues": [
            "easy",
            "medium",
            "difficult"
          ],
          "path": "difficulty",
          "value": "very very hard"
        },
        "kind": "enum",
        "path": "difficulty",
        "value": "very very hard"
      },
      "name": {
        "message": "A tour name must have more or equal then 8 characters",
        "name": "ValidatorError",
        "properties": {
          "message": "A tour name must have more or equal then 8 characters",
          "type": "minlength",
          "minlength": 8,
          "path": "name",
          "value": "Ha"
        },
        "kind": "minlength",
        "path": "name",
        "value": "Ha"
      }
    },
```



```js
const handleValidationErrorDB = err => {
  const errors = Object.values(err.errors).map(el => el.message); // get a string of error messages

  const message = `Invalid input: ${errors.join('. ')}`;

  return new AppError(message, 400);
};
...
 if (error.name === 'ValidationError')
      error = handleValidationErrorDB(error);
...

```
# Errors Outside Express

## 13 Unhandled Promise Rejections

There might also occur errors outside of express.

Ex: MongoDB db connection.

If the database is down for some reason, or for some reason, we cannot log in.

And in that case, there are errors that we have to handle.

If we use the wrong password to DB, we will get an `Unhandled promise rejections`, means  there is a promise that got rejected somewhere in our code.

That rejection has not been handled anywhere.

*in server.js file*
```js
...
const server = app.listen(port, () => {
  console.log(`The server is running in port ${port} !`);
});

//Unhandled promise rejections

process.on('unhandledRejection', err => {
  console.log(err.name, err.message);

  console.log('Unhandled promise rejections!!! Shutting down...');

  // shutdown gracefully with first close the server, and then shutdown the application
  //server.close give server time to finish all the requests before shutdown the app
  server.close(() => {
    process.exit(1);
  });
});
```

## 14 Catching Uncaught Exceptions

All errors, or let's also call them bugs that occur in our synchronous code but are not handled anywhere are called uncaught exceptions.

Because this is for synchronous code, so the order is important.

We need to put this handler at the very top of our code, before we actually require our main application - app.js.

If not, it cannot catch any exception happened in app.js or another files.



```js
process.on('uncaughtException', err => {
  console.log(err.name, err.message);

  console.log('UncaughtException!!! Shutting down...');

  process.exit(1);
});
```

 
