---
id: yn7na9px3jvpjb4srm3pjfp
title: Mongoose
desc: ''
updated: 1663409164558
created: 1663409164558
isDir: false
title_imported: Mongoose
updated_imported: '2020-10-01T09:48:19.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

[TOC]










## [Mongoose 'static' methods vs. 'instance' methods](https://stackoverflow.com/questions/29664499/mongoose-static-methods-vs-instance-methods)

`statics` are the methods defined on the Model. `methods` are defined on the document (instance).

You might use a **static** method like `Animal.findByName`:
```js
const fido = await Animal.findByName('fido');
// fido => { name: 'fido', type: 'dog' }
```
And you might use an instance **method** like `fido.findSimilarTypes`:
```js
const dogs = await fido.findSimilarTypes();
// dogs => [ {name:'fido',type:'dog} , {name:'sheeba',type:'dog'} ]
```

But you wouldn't do `Animals.findSimilarTypes()` because Animals is a model, it has no "type". `findSimilarTypes` needs a `this.type` which wouldn't exist in Animals model, only a document instance would contain that property, as defined in the model.

Similarly you wouldn't¹ do `fido.findByName` because `findByName` would need to search through all documents and `fido` is just ***a*** document. 

¹Well, technically you _can_, because instance does have access to the collection ([`this.constructor`][1] or `this.model('Animal')`) but it wouldn't make sense (at least in this case) to have an instance method that doesn't use any properties from the instance. 

[1]: https://stackoverflow.com/questions/17684673/is-it-possible-to-get-the-model-from-the-document-in-mongoose


## [How to deal with circular dependencies in Moongoose Model](https://github.com/Automattic/mongoose/issues/3826)

https://stackoverflow.com/questions/11904159/automatically-remove-referencing-objects-on-deletion-in-mongodb

https://mongoosejs.com/docs/documents.html



## [Rename document field in Monogoose](https://github.com/Automattic/mongoose/issues/3171)



```js
mongoose
  .connect(databaseConnectionString, {
    useNewUrlParser: true,
    useCreateIndex: true,
    useFindAndModify: false,
    useUnifiedTopology: true,
  })
  .then(() => console.log('DB connection succesful!'))
  .then(() => {
    Tour.updateMany(
      {},
      { $rename: { ratingsAverage: 'rating' } },
      { multi: true, strict: false },
      function (error) {
        if (error) {
          throw error;
        }
        console.log('done!');
      }
    );
  })
  .catch((error) => console.log(error));
```

## Query.explain() method show statistics about query

```js
    const document = await features.query.explain();
```

![df85857a5c9383c0ccdb65fb71c12ace.png](/assets/df85857a5c9383c0ccdb65fb71c12ace-nhd3eo6m9dqb.png)


























`TotalDocsExamined: 10,` Have to scan to all the db to get the result ~~> not good for performance.

To improve this, we need to create `indexes ` on specific fields in the collection that you will search for most the time.

## [The Index](https://docs.mongodb.com/manual/core/index-single/)

In Compass, it show on the Indexes tab

![60111b16d8e8c6ac0a0c4aa128f29d67.png](/assets/60111b16d8e8c6ac0a0c4aa128f29d67-eiryfx8m3wls.png)







Index in Mongo basically an ordered list of all the IDs that get stored somewhere, so whenever documents are queried, Mongo will search the ordered index instead of scan through the whole collection to searching the correct result by look at all the documents one by one. 


By default we have an ID index, and the name field also get index because we set that fields is unique, so Mongo create a unique index for it.


Set the index manually on the model file:

**tourModel.js**
```js
// right after the tourSchema definition
tourSchema.index({ price: 1 }); // 1 = ascending order | -1 = descending order
```

**The query: `URL/?price[lt]=1000` and the result:**

![dcca1a050f12798fe98b211ea4e941a3.png](/assets/dcca1a050f12798fe98b211ea4e941a3-9smx6hakbrp0.png)

## [Compound index](https://docs.mongodb.com/manual/core/index-compound/)
```js
tourSchema.index({ price: 1, ratingsAverage: -1}); 
```



**The query: `URL/?price[lt]=1000&ratingsAverage[gte]=4.8` and the result:**


![71b0b75c348cc1f79cf37e53c4173d7d.png](/assets/71b0b75c348cc1f79cf37e53c4173d7d-lspqcjvcwaa3.png)




[@Docs](https://mongoosejs.com/docs/api/query.html#query_Query-explain)


## Turn off validation for Model.create() - for quick upload backup data

```js
await Tour.create(users, {validateBeforeSave: false});
```


## Turn off validation for Model.insertMany() - for quick upload backup data

```js
await Tour.insertMany(tours, { lean: true });
await User.insertMany(users, { lean: true });
await Review.insertMany(reviews, { lean: true });
```

[options.lean «Boolean» = false] if true, skips hydrating and validating the documents. This option is useful if you need the extra performance, but Mongoose won't validate the documents before inserting.



## Plugin for Validate the Existence of An Object Id in Referencing?

I'm using [mongoose-id-validator](https://www.npmjs.com/package/mongoose-id-validator). Works good

```js
var mongoose = require('mongoose');
var idValidator = require('mongoose-id-validator');

var ReferencedModel = new mongoose.Schema({name: String});

var MySchema = new mongoose.Schema({
  referencedObj : { type: mongoose.Schema.Types.ObjectId, ref: 'ReferencedModel'},
  referencedObjArray: [{ type: mongoose.Schema.Types.ObjectId, ref: 'ReferencedModel' }]
});

MySchema.plugin(idValidator);
```

[@Stackoverflow](https://stackoverflow.com/questions/18516610/does-mongoose-actually-validate-the-existence-of-an-object-id)

## Geospatial Data in MongoDB
https://docs.mongodb.com/manual/geospatial-queries/



## Do not declare instance methods using ES6 arrow functions ( => )
Do not declare methods using ES6 arrow functions (=>). Arrow functions explicitly prevent binding this, so your method will not have access to the document and the above examples will not work.

## Các cách update data trên Mongoose

There are 2 ways to update data to db
- 1: using `User.findById` and `User.save()`
    - dùng để update data có password ( có phần check password)
    
- 2: using User.findByIdAndUpdate
    ```
      const updatedUser = await User.findByIdAndUpdate(req.user.id, req.body, {
        new: true, //  true to return the modified document rather than the original.
        runValidators: true //  runs update validators on this command.
    
        // Update validators validate the update operation against the model's schema.
        });
    ```
    - Dùng khi update các non-sensitive data, ko có password ( bypass phần check password)
    - Chú ý phải lọc req.body nếu có password hay các filed nhạy cảm trước đó
    



## Update Validators
Mongoose also supports validation for `update()`, `updateOne()`, `updateMany()`, and `findOneAndUpdate()` operations.

Update validators are off by default - you need to specify the `runValidators` option.

To turn on update validators, set the runValidators option for `update()`, `updateOne()`, `updateMany()`, or `findOneAndUpdate()`.

Be careful: update validators are off by default because they have several caveats.

```js
var toySchema = new Schema({
  color: String,
  name: String
});

var Toy = db.model('Toys', toySchema);

Toy.schema.path('color').validate(function (value) {
  return /red|green|blue/i.test(value);
}, 'Invalid color');

var opts = { runValidators: true };
Toy.updateOne({}, { color: 'not a color' }, opts, function (err) {
  assert.equal(err.errors.color.message,
    'Invalid color');
});
```

[@source](https://mongoosejs.com/docs/validation.html#update-validators)

## Beware of validation and middleware bypass

Don't Use any of these function:

```js
User.create();

User.findOneAndUpdate();
User.findByIdAndUpdate();

User.remove();
User.findOneAndRemove();
User.findByIdAndRemove();
```
[@source](https://twm.me/correct-way-to-use-mongoose/)


## Turn off the validator when we need to save something not suit the schema
Eg : PasswordComfirm : ask user to add it but will delete after all.

await user.save({ validateBeforeSave: false });

## Connect to MongoDB

```js
mongoose
 .connect('mongodb://127.0.0.1:27017/myapp', {  							
	useNewUrlParser: true,
    useCreateIndex: true,
    useFindAndModify: false,
    useUnifiedTopology: true
}).then( () => console.log('DB connection succesful!') );
```


## `Model.insertMany(docs,  options,  callback) => {promise}`

Shortcut for validating an array of documents and inserting them into MongoDB if they're all valid.

This function is faster than  `.create()`  because it only sends one operation to the server, rather than one for each document.

Mongoose always validates each document  **before**  sending  `insertMany`  to MongoDB. So if one document has a validation error, no documents will be saved, unless you set  [the  `ordered`  option to false]

```js
Tour.insertMany(tours, { ordered:  false }, err  => {
console.log(err);
});
```
[@source](https://mongoosejs.com/docs/api.html#model_Model.insertMany)


## Using `findOneAndDelete()` instead of `findOneAndRemove`
**You should use `findOneAndDelete()` unless you have a good reason not to!**

This function differs slightly from `Model.findOneAndRemove()` in that `findOneAndRemove()` becomes a MongoDB  `findAndModify()`  command, as opposed to a `findOneAndDelete()` command. For most mongoose use cases, this distinction is purely pedantic.

[@source](https://mongoosejs.com/docs/api.html#model_Model.findOneAndDelete)

## Aggregation Pipeline

[https://docs.mongodb.com/manual/core/aggregation-pipeline/](https://docs.mongodb.com/manual/core/aggregation-pipeline/)

[https://studio3t.com/knowledge-base/articles/mongodb-aggregation-framework/](https://studio3t.com/knowledge-base/articles/mongodb-aggregation-framework/)

[https://viblo.asia/p/aggregation-va-mot-so-aggregation-pipeline-stages-trong-mongodb-1VgZvXLm5Aw](https://viblo.asia/p/aggregation-va-mot-so-aggregation-pipeline-stages-trong-mongodb-1VgZvXLm5Aw)



## If a query has some wrong value, it will skip that value and pass successful at all

if using wrong `ratingAverage` instead of `ratingsAverage`
```js
exports.aliasTopFiveTours = (req, res, next) => {
  req.query.limit = '5';
  req.query.sort = '-ratingsAverage,price';
  req.query.fields = 'name,price,ratingsAverage,summary,difficulty';
  next();
};

//OUTPUT - success without ratingsAverage fields
// { "_id": "5d99ad17cbc74c33e46d9c58",  "name": "The Northern Lights",  "difficulty": "easy",  "price": 1497,  "summary": "Enjoy the Northern Lights in one of the best places in the world"  },
```


## Hide some sensitive fileds permanently in our Schema.
*in tourModel.js*
```js
 createAt:{
	 type: Date,
	 default: Date.now(),
	 select: false // hide this fields so the client never get that,
 }
```


## exclude id in Mongoose when select fields
[@mongodb docs](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/#suppress-id-field)
[moongose](https://github.com/Automattic/mongoose/issues/1534)

## `Model.insertMany(docs,  options,  callback) => {promise}`

Shortcut for validating an array of documents and inserting them into MongoDB if they're all valid.

This function is faster than  `.create()`  because it only sends one operation to the server, rather than one for each document.

Mongoose always validates each document  **before**  sending  `insertMany`  to MongoDB. So if one document has a validation error, no documents will be saved, unless you set  [the  `ordered`  option to false]

```js
Tour.insertMany(tours, { ordered:  false }, err  => {
console.log(err);
});

```
[@source](https://mongoosejs.com/docs/api.html#model_Model.insertMany)

## Using `findOneAndDelete()` instead of `findOneAndRemove`
**You should use `findOneAndDelete()` unless you have a good reason not to!**

This function differs slightly from `Model.findOneAndRemove()` in that `findOneAndRemove()` becomes a MongoDB  `findAndModify()`  command, as opposed to a `findOneAndDelete()` command. For most mongoose use cases, this distinction is purely pedantic.

[@source](https://mongoosejs.com/docs/api.html#model_Model.findOneAndDelete)

## Operation Buffering

Mongoose lets you start using your models immediately, without waiting for mongoose to establish a connection to MongoDB.

```javascript
mongoose.connect('mongodb://localhost:27017/myapp', {useNewUrlParser: true});
var MyModel = mongoose.model('Test', new Schema({ name: String }));
// Works
MyModel.findOne(function(error, result) { /* ... */ });
```

That's because mongoose buffers model function calls internally. This buffering is convenient, but also a common source of confusion. Mongoose will  _not_  throw any errors by default if you use a model without connecting.

```javascript
var MyModel = mongoose.model('Test', new Schema({ name: String }));
// Will just hang until mongoose successfully connects
MyModel.findOne(function(error, result) { /* ... */ });

setTimeout(function() {
  mongoose.connect('mongodb://localhost:27017/myapp', {useNewUrlParser: true});
}, 60000);
```
[@sourse](https://mongoosejs.com/docs/connections.html#buffering)

