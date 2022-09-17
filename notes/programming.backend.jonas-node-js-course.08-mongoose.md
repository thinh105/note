---
id: 90ypsv2nm1zgu1g2ndr5v2d
title: 08 Mongoose
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
title_imported: 08 Mongoose
updated_imported: '2020-08-18T09:49:49.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

[TOC]
# 2 Connect Database with the Express App


### Get Connection String to connect to NodeJS application
 
 - get the string in Atlas
 - paste into env File
 - *for hosted db*
 ```
	 DATABASE = .....
	 change password to PASSWORD
	 admin = natour
```
- *for local database:*
	```
	DATABASE_LOCAL = mongodb://127.0.0.1:27017/natour
	```


### Install Mongoose
```bash
npm i mongoose
```
in server.js file

```js
const mongoose = require('mongoose');

// for the hosted db
const DB = process.env.DATABASE.replace(
	'<PASSWORD>',
	process.env.DATABASE_PASSWORD
);

// for the local db
// const DB = process.env.DATABASE_LOCAL	

mongoose
 .connect(DB, {  							
	useNewUrlParser: true,
    useCreateIndex: true,
    useFindAndModify: false,
    useUnifiedTopology: true
}).then( () => console.log('DB connection succesful!') );

//	}).then con=>{
	// console.log(con.connections);				
	// console.log('DB connection succesful!');
	// });

```

# 3-5 _ Create a Simple tour Model

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node,js.
It's a higher level of abstraction. ( same Express and Node.js

![enter image description here](http://i.imgur.com/99wqmeV.png)

Mongoose are all about models - blueprint that we use to create documents.
Models in MongoDB is a bit like Classes in JS - kind of blueprint to create objects

![enter image description here](http://i.imgur.com/Uiv9afG.png)
  
**in server.js file**

*Create a Schema*
```js 
 const tourSchema = new mongoose.Schema({
	name: {
		type: String,
		required: [true, 'A tour must have a name'],
		unique: true
		},
	rating: {
		type: Number,
		default: 4.5
	}
	price: {
		type: Number,
		required: [true, 'A tour must have a price']
	}
});
```

*Create a Model*
```js	 
const Tour = mongoose.model('Tour', tourSchema); // convention: Use Uppercase on Model Names and Variables.

```

*Create a new Document out of a Tour Model*
```js
 const testTour = new Tour({
	 name: 'Ha Long Bay',
	 rating: 4.7,
	 price: 696
});

testTour.save().then(doc => {
	console.log(doc);
}).catch( err => console.log('ERROR :',err));
``` 
>Basically the same of creating an Object out of a class 

# 6-7 MVC
## Model - View - Controller Architecture

- Model - Business Logic
- Controller - Application Logic
- View - Presentation Logic

![enter image description here](http://i.imgur.com/q6Rwqmx.png)

One of the big goals of MVC is to separate business logic from application logic.

![enter image description here](http://i.imgur.com/1z8B4SC.png)

### Refactoring for MVC
+ Create a Controllers folder
	+ Create a Controller.js file
+ Create a Models folder
	+ Create a Model.js File
	+ Move the Schema, Model declaration to there
	+ Export to the original FIle `module.exports = Tour`
+ In Controller.js File
	+ Import the Model module to there
	+ Remove the old parts that connect to the JSON file, now we have the DB
	+ Comment some old part still useful but involve with the JSON part
	+ remove the CheckID, checkBody function, remove module FS

# 8-11_ CRUD Operation

## Creating Documents in DB
CreateTour function in tourController
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

When send post data, all the fields that not in our Schema will be not put in the db.

*post*
```js
{ 
	"name": "Ha Long Boi",
	"duration": "3 days 2 nights",
	"difficulty": "Easy",
	"price": 100,
	"rating": 4.7
}
```
*db get*
```js
{
	"name": "Test tour",
	"price": 100,
	"rating": 4.7
}
```
 Because the Tour Schema don't have `duration` and `difficulty` field
 Everything else that is not in our Schema is simple ignored.
 
 ## Reading Document
  
```js
exports.getAllTours = async (req, res) => {
  try {
    const tours = await Tour.find();

    res.status(200).json({
      status: 'success',
      result: tours.length,
      data: { tours }
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};

exports.getTour = async (req, res) => {
  try {
    const tour = await Tour.findById(req.params.id);

    res.status(200).json({
      status: 'success',
      data: { tour }
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};
``` 
	 
 
 ## Updating Document
 
```js
exports.updateTour = async (req, res) => {
  try {
    const tour = await Tour.findByIdAndUpdate(req.params.id, req.body, {
        new: true, // return the new Update document to client
    runValidators: true // run the validator
    });
     res.status(200).json({
      status: 'success',
      dataUpdate: { tour }
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: err
    });
  }
};
```

 ## Delete Document
```js
exports.deleteTour = async (req, res) => {
  try {
    await Tour.findByIdAndDelete(req.params.id);
    // in RESTful API, It is common practice not send anything back to client when deleted
    res.status(204).json({
      status: 'success',
      data: null
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: err
    });
  }
};
```

# 14-19 Making the API better

## Making the API better : Filtering - Sorting - Limiting - Pagination - Aliasing

These lessons  manipulates the Mongoose Queries methods to create a better API

### Filtering in Mongoose

GET: `127.0.0.1:6969/api/v1/tours?duration=5&difficulty=easy`

Express parses that string into `req.query`
```js
console.log('req.query');
// { duration: '5',difficulty: 'easy' }
```

In Mongoose, there are 2 way to write db queries.
*Query in MongoDB*
```js
const tours = await Tour.find({ duration: '5',difficulty: 'easy' } );
```

*Query in Mongoose*
```js
const tours = await Tour.find()
	.where('duration')
	.equals(5)
	.where('difficulty')
	.equals('easy');
```

### Exclude some special field names on query string
*in tourController.js > getAllTours Module*
```js

// BUILD QUERY

const queryObj = {...req.query}; // using destructuring to hard copy in ES6
//we need hard copy because when we set a varialbe to an object, that new variable just be a reference to that original object.
// JS doesn't have a build in function for that

const excludedFields = ['page', 'sort', 'limit', 'fields'];
excludedFields.forEach(el => delete queryObj[el]);

const query = Tour.find(queryObj);

//EXECUTE QUERY
const tours = await query;

//SEND RESPONSE
res.status...
```
###  Advanced Filtering in Mongoose

GET: `127.0.0.1:6969/api/v1/tours?duration[gte]=5&difficulty=easy`

Express parses that string into `req.query`
```js
console.log('req.query');
// { duration: { gte:'5'},difficulty: 'easy' }
```

*in tourController.js*
```js
//BUILD QUERY

// 1A) Filtering
...
// 1B) Advanced filtering

// Express parse query  { duration: { gte:'5'},difficulty: 'easy' }
// MongoDB query { duration: { $gte:'5'},difficulty: 'easy' }

// string need to replace:  gte, gt, lte, lt

let queryStr = JSON.stringify(queryObj);
queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, match => `$${match}`);
// \b: match the exact word - g change all the time
// using regular expressions -> google stackoverflow to find out

console.log(JSON.parse(queryStr));

let query = Tour.find(JSON.parse(queryStr));
```

### Sorting in Mongoose

GET: `127.0.0.1:6969/api/v1/tours?sort=price`
Express parses that string into `req.query`
```js
console.log('req.query');
// { sort: 'price' }
```

*in tourController.js*
```js
// 2) Sorting
if(req.query.sort) {
query = query.sort(req.query.sort) // sort is the Mongoose method
} 
```

GET: `127.0.0.1:6969/api/v1/tours?sort=price` in an ascending order
GET: `127.0.0.1:6969/api/v1/tours?sort=-price` Mongoose will automatic sort in a descending order

In Mongoose, `sort('price ratingsAverage')`for sorting more than 1 field
So we can use comma 'price,ratingsAverage' in the URL and use JS to replace this comma to a space 'price ratingsAverage'

GET: `127.0.0.1:6969/api/v1/tours?sort=price,ratingsAverage`

*in tourController.js*
```js
// 2) Sorting
if(req.query.sort) {
const sortBy = req.query.sort.split(',').join(' ');
query = query.sort(sortBy);
} else {
query = query.sort('-createAt');
}
```
### Select Fields in Mongoose

GET: `127.0.0.1:6969/api/v1/tours?fields=name,duration,difficulty,price`

*in tourController.js*
```js
// 3 - Field Selection

//tours?fields=name,duration,difficulty,price

if (req.query.fields) {
 const fields = req.query.fields.split(',').join(' ');
 query = query.select(fields);
} else query = query.select('-__v'); //using - to exlcude some field
```
We can hide some sensitive fileds permanently in our Schema.
*in tourModel.js*
```js
 createAt:{
	 type: Date,
	 default: Date.now(),
	 select: false // hide this fields so the client never get that,
 }
```
### Pagination in Mongoose

GET: `127.0.0.1:6969/api/v1/tours?page=2&limit=10`

*in tourController.js*

```js
 // 4 - Pagination

//tours?page=2&limit=10
// page = 2 & limit = 10
// page1: 1-10, page2: 11-20, page3: 21-30

const page = parseInt(req.query.page, 10) || 1;
const limit = parseInt(req.query.limit, 10) || 5;
const skip = (page - 1) * limit;

query = query.skip(skip).limit(limit);

if (req.query.page) {
  const numTours = await Tour.countDocuments();
  if (skip >= numTours) throw new Error('This page does not exist!!!');
}
```
 
### Aliasing in Mongoose

For the top best 5 tour:
GET: `127.0.0.1:6969/api/v1/tours?limit=5&sort=-ratingsAverage,price` is too complicated

What we want is some thing like this:

GET: `127.0.0.1:6969/api/v1/tours/top-5-tours`

*in tourRouter.js*
```js
router 
  .route('/top-five-tours') // to avoid conflix with route('/:id'), place this router above router /:id
  .get(tourController.aliasTopFiveTours, tourController.getAllTours);
```

*in tourController*
```js
exports.aliasTopFiveTours = (req, res, next) => {
  req.query.limit = '5';
  req.query.sort = '-ratingsAverage,price';
  req.query.fields = 'name,price,ratingAverage,summary,difficulty';
  next();
};
```

# 20 Refactoring API features

## create APIFeature class 

Some hard things this video mentions about :
	+ chain of methods
	+ the Model.find()

```js
class APIFeatures {
	constructor(query, queryString){
	this.query = query;
	this.queryString = queryString;
	}

	filter() {
	  // change req.query = this.queryString
	  // change query = Tour.find ... to this.query.find(JSON.parse(this.QueryString)
	} 
}

//...

//EXECUTE QUERY

const features = new APIFeatures()
++ 

```

# 21-22_ Aggregation pipeline
## Aggregation pipline
ex: calculate average, minimum, maximum value, etc
this is MongoDB feature.

We can use `Model.Aggregate`like a regular query, but we can manipulate the data in a couple different steps.



+ Define a new route to use that function ...
```js
router.route('/tour-stats').get(tourController.getTourStats);
```
+ Create a new function to calculate a couple statistics about our tours
```js
	exports.getTourStats = async (req,res ) => {
	try {
		const stats = await Tour.aggregate([ // pass in an array of stages ]);
		res.status(200).json({ ... })
		} catch (err) { res.status(404).json ...
	}		
```

### Staged
visit MongoDB docs > Aggregation > Reference > Opetators > Aggregation Pipeline Stages
[link](https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/) 

```js
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATIONS)
```


#### Match & Group & Sort
$match : select/filter certain documents
$group: allows us to group documents together 

*array of stages*
```js
	 {
	 $match: { ratingsAverage: {$gte: 4.5} }
	 },
	 {
	 $group: { 
			_id: '$difficulty', // decide which field will be grouped to show data for that group.
			num: { $sum: 1 }, //   for each document go through this pipeline, 1 will be added to this num counter.
			numRating: { $sum: '$ratingQuantity' },
			avgRating: { $avg: '$rating' },
			avgPrice: { $avg: '$price' },
			minPrice: { $min: '$price' },
			maxPrice: { $max: '$price' }
		 }
	 },
	 {
		 $sort: { avgPrice: 1 } // 1 for ascending
	 },
	 {
		 $match: { _id: { $ne: 'Easy'} } // ne = not equal
	 }

//output:
// we have statistics for each group of different value of fields specified in _id (difficulty : easy medium and hard)
```

We can do some operations with `_id` : 
```js
_id: {$toUpper: '$difficulty' },
```

#### Unwind & Project 
*Business Requirement* : Implement a function to calculate the busiest month of a given year
Means, Calculate how many tours start in each of the month of the given year.

*Define a new route to use that function *
```js
router.route('/monthly-plan/:year').get(tourController.getMonthlyPlan);
```
*

*array of stages*
```js
{
        $unwind: '$startDates'
      },
      {
        $match: {
          startDates: {
            // Date comparison on MongoDbga
            $gte: new Date(`${year}-01-01`), 
            $lte: new Date(`${year}-12-01`)
            // Date() is Mongodb Object Constructors and Methods
          }
        }
      },
      {
        $group: {
          _id: { $month: '$startDates' },
          // $month (aggregation) Returns the month of a date as a number between 1 and 12.
          numTourstarts: { $sum: 1 },
          tours: { $push: '$name' }
          // Returns an array of all values that result from applying an expression
          // to each document in a group of documents that share the same group by key.
          // $push is only available in the $group stage.
        }
      },
      {
        $addFields: {
          month: '$_id'
        }
      },
      {
        $project: {
          _id: 0
        }
      },
      {
        $sort: {
          numTourstarts: -1
        }
      }
```
# 27-28 Data Validation Validator

## Data validation

Fat model - thin controller Philosophy
 
### Built-in validators
 
 *in Schema section*
```js
name: {
type: String,
required: [true, 'A tour must have a name'], //built-in validators
unique: true,
trim: true,
maxlength: [40,'error message'], //built-in validators
minlength: [10, 'error message'], //built-in validators
},
ratingsAverage: {
type: Number,
default: 3,
min:[1,'error message'], //built-in validators - works with number and dates
max:[5,'error message'] //built-in validators - works with number and dates
},
difficulty: {
type: String,
required: [true, 'error message'], //built-in validators
enum: { //built-in validators - only works with string
	value: ['easy','medium','difficult'],
	message: 'error message'  
}
```

### Custom validators

it is a real function that return `true` or `false` 

  *in Schema section*
> `this` only work on NEW document creation
this validator below is not working with the update action 

```js
priceDiscount: {
type: Number,
validate: function(val) {
// val is the value that was inputted
// Cannot use arrow function if we need `this` variable to point to current document
// this only work in new document - not update
	return val < this.price; // 100 < 200
},
message: 'error message {VALUE}' // {VALUE} is the value that was inputted in Mongoose 
```

### Validator.js library
 [https://validatejs.org/](https://validatejs.org/)

this library validates and sanitizes strings only.






.



 
