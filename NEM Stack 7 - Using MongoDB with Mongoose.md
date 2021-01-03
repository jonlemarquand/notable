---
title: NEM Stack 7 - Using MongoDB with Mongoose
created: '2020-04-09T06:59:03.876Z'
modified: '2020-04-09T07:50:52.312Z'
---

# NEM Stack 7 - Using MongoDB with Mongoose

* Get the string from atlas and put it in `config.env`
* Local mongo db: `DATABASE_LOCAL=mongodb://localhost:27017/natours`
  * Need to keep the mongod process going at all times.
* `npm i mongoose`
* Connect the database:


```javascript
const mongoose = require('mongoose');

const DB = process.env.DATABASE.replace('<PASSWORD>', process.env.DATABASE_PASSWORD);
mongoose.connect(DB, {
  useNewUrlParser: true,
  useCreateIndex: true,
  useFindandModify: false
}).then(() => {console.log('DB connection successful')})
```
## What is Mongoose?

* Mongoose is a way to write javascript code to work with MongoDB and Nodejs.
* Mongoose allows for rapid and simple development of mongoDB database interactions.
* Features: schemas to model data and relationships, easy data validation, simple query API, middleware etc.
* Mongoose schema: Where we model our data, by describing the structure of the data, default values, validation.
* Mongoose model: a wrapper for the schema, providing an interface to the database for CRUD operations.

## Creating a simple tour model

* Models are a bit like classes in javascript. They are the basis for documents.

```javascript

const tourSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'A tour must have a name'],
    unique: true
  }
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
* This is the basic schema. 
* To create a tour: `const Tour = mongoose.model('Tour', tourSchema);`

## Creating Documents and Testing the Model

* This is a new document being created out of the model:

```javascript

const testTour = newTour({
  name: 'The Forest Hiker',
  rating: 4.7,
  price: 497
});

 // This will save the testTour to the database
testTour.save().then(doc => {
  console.log(doc);
}).catch(err => {
  console.log('Error:' err);
})

```
