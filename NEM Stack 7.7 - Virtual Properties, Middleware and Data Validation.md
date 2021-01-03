---
title: 'NEM Stack 7.7 - Virtual Properties, Middleware and Data Validation'
created: '2020-04-12T07:39:39.915Z'
modified: '2020-04-12T09:35:15.811Z'
---

# NEM Stack 7.7 - Virtual Properties, Middleware and Data Validation

## Virtual Properties

* Virtual properties that we can define on our schema but they will not be saved on to the database.
* Fields that can be derived from one another.
  * Stored in miles but also want them in kilometers.
* The virtual property will be created every time we get some data out of the database.
* Used a function because arrow functions don't get their own this keyword.

```javascript

tourSchema.virtual('durationWeeks').get(function() {
  return this.duration / 7
})

}, {
  toJSON: { virtuals: true },
  toObject: { virtuals: true}
});

```
* This last section is the settings object of the schema that says when the toJSON is called, virtual objects will be true.
* These properties aren't part of the database, so can't be queried.

## Mongoose Middleware

### Document Middleware

* A middleware that can act on the currently accessed document.
* Only runs on save and create

```javascript

//DOCUMENT MIDDLEWARE: runs before .save() and .create()

tourSchema.pre('save', function(next) {
  this.slug = slugify(this.name, { lower: true });
  next();
});

tourSchema.post('save', function(doc, next) {
  console.log(doc);
  next();
})

```
* Using slugify package
* Post middleware are executed after all pre middleware are done.


### Query Middleware

* The this query will now point at the current query, rather than the current document.
* The regular expression just means: all the hooks that start with 'find' such as 'findOne'
* A pre-find hook:

```javascript

secretTour: {
  type: boolean,
  default: false,
}

// QUERY MIDDLEWARE

tourSchema.pre(/^find/, function(next) {
//tourSchema.pre('find', function(next) {
  this.find({ secretTour: {$ne: true} });
  this.start = Date.now();
  next();
});

tourSchema.post(/^find/, function(docs, next) {
  console.log(`Query took ${Date.now() - this.start} milliseconds!`)
  console.log(docs);
  next();
})

```

### Aggregation Middleware

```javascript 
tourSchema.pre('aggregate', function(next) {
  this.pipeline().unshift({ $match: { secretTour: {$ne: true} } });
  next();
})
```

* The pipeline is an array, so we use standard javascript methods to add stages to them.


## Data Validation with Mongoose

### Built-In Validators

* Are the values the right values, and have they been entered.
* Sanitisation: Is the code being entered clean?
* `required: [true, 'A tour must have a name']` is already a form of in-build validation.
* For strings: `maxlength: [40, 'A tour name must have fourty character or less']`
  * This also works for minlength.
* For numbers:
  * `min: [1, 'Rating must be above 1.0],`
  * `max: [5, 'Rating must be below 5.0],`
* Restrict values:
  * enum: { 
      values: ['easy', 'medium', 'difficult'],
      message: 'Difficulty is either: easy, medium, difficult'
    }

### Custom Validators

* A simple function which returns true or false

```javascript

price: {
  type: Number,
  required: [true, 'A tour must have a price']
},
priceDiscount: {
  type: Number,
  validate: {
    validator: function(val) {
    return val < this.price;
  },
  message: 'Discount price ({VALUE}) should be below regular price',
}

```
* This function will not work on updating a value (just because of the use of the `this` variable)
