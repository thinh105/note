---
id: ykifwelvqo6caxnkhrctybt
title: Express
desc: ''
updated: 1663409164558
created: 1663409164558
isDir: false
title_imported: Express
updated_imported: '2020-07-24T11:43:01.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

[TOC]












## Nested Routes with Express

You want to create a review base on tourId from parameter.

```http
GET /tour/1234/reviews
POST /tour/1234/reviews
```

This is about Reviews but it was sent to tourRouter

You can nest routers by attaching them as middleware on an other router, with or without params.

You must pass {mergeParams: true} to the child router if you want to access the params from the parent router.

**tourRouter**
```js
const reviewRouter = require('./reviewRouter');

/*...*/

// Nested Routes with Express
router.use('/:tourId/reviews', reviewRouter); 
```

the same on **app.js**
```js
// 2 ROUTES

app.use('/api/v1/tours', tourRoutes);
app.use('/api/v1/users', userRoutes);
app.use('/api/v1/reviews', reviewRoutes);
```


**reviewRouter**
```js
const router = express.Router({ mergeParams: true });
// allow access to the parameters from another routers

router
  .route('/')
  .get(reviewController.getAllReviews)
  .post(authController.protect, reviewController.createReview);
```

**reviewController**
```js
exports.createReview = catchAsync(async (req, res, next) => {
  
  // Allow nested routes

	//get tourId from parameter
  if (!req.body.tour) req.body.tour = req.params.tourId; 
  
  // get user id from authentication controller
  if (!req.body.user) req.body.user = req.user.id; 


  // normal part
  const newReview = await Review.create(req.body);
  res.status(201).json({
    status: 'success',
    data: { review: newReview }
  });
});
```

Result: 

GET /tour/1234/reviews

POST /tour/1234/reviews

~~> the reviewRouter take control


## Example in Express

https://github.com/expressjs/express/tree/master/examples

## Order in Express is very Important

The order of your middleware stack is important - make sure your requests flow through in the proper order.

When the Express.js app is running, it's listening to requests.

Each incoming request is processed according to a defined chain of middleware and routes, starting from top to bottom.

This aspect is important in controlling the execution flow. For example,routes/middleware that are higher in the file have precedence over the lower definitions.

**Set dotenv below app express then app cannot get the  environment variables.**

*WRONG*

```js
const app = require('./app');
require('dotenv').config({ path: './config.env' }); 
```

*RIGHT*
```js
require('dotenv').config({ path: './config.env' });
const app = require('./app');

```


*WRONG ROUTE ORDER*

```js
router
  .route('/:id')
  .get(tourController.getTour)
  .patch(tourController.updateTour)
  .delete(tourController.deleteTour);

router
  .route('/top-five-tours')
  .get(tourController.aliasTopFiveTours, tourController.getAllTours);
  
//Output for /top-five-tours
{
  "status": "fail",
  "message": {
    "message": "Cast to ObjectId failed for value \"top-five-tours\" at path \"_id\" for model \"Tour\"",
    "name": "CastError",
    "stringValue": "\"top-five-tours\"",
    "kind": "ObjectId",
    "value": "top-five-tours",
    "path": "_id"
  }
}
```
<br>

*CORRECT ROUTE ORDER*

```js

router
  .route('/top-five-tours')
  .get(tourController.aliasTopFiveTours, tourController.getAllTours);
  
router
  .route('/:id')
  .get(tourController.getTour)
  .patch(tourController.updateTour)
  .delete(tourController.deleteTour);

//Output for /top-five-tours
{
  "status": "success",
  "result": 5,
  "data": { ...}
}

```

<br>

*WRONG MIDDLEWARE ORDER*
```js

app.use((err, req, res, next) => {
  err.statusCode = err.statusCode || 500; // default is 500 - internal server error
  err.status = err.status || 'error';

  res.status(err.statusCode).json({
    status: err.status,
    message: err.message
  });
});


app.all('*', (req, res, next) => {
  const err = new Error(`Can't find '${req.originalUrl}' on this server!`); // ( = error.message)
  err.status = 'fail';
  err.statusCode = 404;

  next(err);
});

```
<br>

*CORRECT MIDDLEWARE ORDER*
```js
app.all('*', (req, res, next) => {
  const err = new Error(`Can't find '${req.originalUrl}' on this server!`); // ( = error.message)
  err.status = 'fail';
  err.statusCode = 404;

  next(err);
});

app.use((err, req, res, next) => {
  err.statusCode = err.statusCode || 500; // default is 500 - internal server error
  err.status = err.status || 'error';

  res.status(err.statusCode).json({
    status: err.status,
    message: err.message
  });
});
```







## Basic Routing
[Basic Routing](https://expressjs.com/en/starter/basic-routing.html)

[https://expressjs.com/en/guide/routing.html](https://expressjs.com/en/guide/routing.html)



## Middleware
[@link](https://expressjs.com/en/guide/using-middleware.html)

_Middleware_ functions are functions that have access to the [request object](https://expressjs.com/en/4x/api.html#req) (`req`), the [response object](https://expressjs.com/en/4x/api.html#res) (`res`), and the next middleware function in the application’s request-response cycle.

## express.json vs bodyParser.json

Earlier versions of Express used to have a lot of middleware bundled with it. bodyParser was one of the middlewares that came it. When Express 4.0 was released they decided to remove the bundled middleware from Express and make them separate packages instead. The syntax then changed from  `app.use(express.json())`  to  `app.use(bodyParser.json())`  after installing the bodyParser module.

bodyParser was added back to Express in release 4.16.0, because people wanted it bundled with Express like before. That means you don't have to use  `bodyParser.json()`  anymore if you are on the latest release. You can use  `express.json()`  instead.

The release history is for 4.16.0 is  [here](https://github.com/expressjs/express/releases/tag/4.16.0)  for those who are interested, and the pull request  [here](https://github.com/expressjs/express/pull/3423).

[@source](https://stackoverflow.com/questions/47232187/express-json-vs-bodyparser-json)

[Express 4](https://expressjs.com/en/guide/migrating-4.html)


# [Error handling in Express with async await](https://medium.com/@Abazhenov/using-async-await-in-express-with-node-8-b8af872c0016)

## [Express Async Handler package npm](https://www.npmjs.com/package/express-async-handler)

https://github.com/Abazhenov/express-async-handler/

```js
const asyncUtil = fn =>
function asyncUtilWrap(...args) {
  const fnReturn = fn(...args)
  const next = args[args.length-1]
  return Promise.resolve(fnReturn).catch(next)
}

module.exports = asyncUtil
```

**Usage:**
```js
const asyncHandler = require('express-async-handler')

express.get('/', asyncHandler(async (req, res, next) => {
	const bar = await foo.findAll();
	res.send(bar)
}))
```

**Without express-async-handler**

```js
express.get('/',(req, res, next) => {
    foo.findAll()
    .then ( bar => {
       res.send(bar)
     } )
    .catch(next); // error passed on to the error handling route
})
```


---


https://anonystick.com/blog-developer/async-await-error-handling-in-express-2019053123997894.jsx

Tạo một function thứ 3 đa số nằm trong file Utils á:

**Util.js**

```js
module.exports = fn => (req, res, next) => {
    Promise.resolve(fn(req, res, next))
        .catch(next);
};
```
**Sử dụng nè: Controller.js**

```js
const getUser = async (req, res, next) => {
    const { id } = req.params;
    const user = await User.findById(id); // f.ex. mongoose findById method
}
```

**Routes.js**

```js
const asyncMiddleware = require('Util');
router.get("/user/:id", asyncMiddleware(getUser));
```

https://zellwk.com/blog/async-await-express/

https://nemethgergely.com/error-handling-express-async-await/

https://dev.to/geoff/writing-asyncawait-middleware-in-express-6i0
