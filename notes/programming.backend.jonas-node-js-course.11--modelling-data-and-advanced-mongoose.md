---
id: bsefdeyl3sew4c3b1515apl
title: 11  Modelling Data and Advanced Mongoose
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: 11. Modelling Data and Advanced Mongoose
updated_imported: '2020-04-02T14:52:29.000Z'
created_imported: '2020-03-09T04:23:13.000Z'
---

# MongoDB Data Modelling

![bb0df12d32158b91165836dbc6e752fa.png](/assets/bb0df12d32158b91165836dbc6e752fa-l1oo4fx9wui0.png)

![37594972d9db8f362db6e807e681d9af.png](/assets/37594972d9db8f362db6e807e681d9af-98eurpzalcuk.png)

![aa30ca39c140bf07fe9d25d0e55ce57f.png](/assets/aa30ca39c140bf07fe9d25d0e55ce57f-oevwpi7bx0z4.png)

![407e8aecbb3ad26763503003e24961b7.png](/assets/407e8aecbb3ad26763503003e24961b7-dm4ajhkru6cs.png)

</br>

# Natour Data Model

![1efd6ebd3576c9a6422a749e9dbf575a.png](/assets/1efd6ebd3576c9a6422a749e9dbf575a-wx3ghqnukrqj.png)

# Embedding data

```js
tourSchema.pre('save', async function(next){
	const guidesPromises = this.guides.map(async id => await User.findById(id));
	this.guides = await Promise.all(guidesPromises);
	next();
});
```

# Child Referencing Data

```js
guides: [
    {
      type: mongoose.Schema.ObjectId,
      ref: 'User'
    }
  ]
```

# Get the referencing Data

We use `populate` to get access to the referenced data, replace ref data with actual related data.

Populate in query, not in actual database

Using `Populate` create a new query, this might affect the performance 


**in tourController.js**
```js
const tour = await Tour.findById(req.params.id).populate('guides');
```

We can use query middleware to automatically populate everytime we send the find query

**in tourModel.js**
```js
tourSchema.pre(/^find/, function(next) {
  this.populate({
    path: 'guides',
    select: '-__v -passwordChangedAt -passwordResetExpires -passwordResetToken'
  });
  next();
});
```


