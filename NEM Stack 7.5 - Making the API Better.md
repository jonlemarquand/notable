---
title: NEM Stack 7.5 - Making the API Better
created: '2020-04-11T08:29:27.634Z'
modified: '2020-04-11T11:02:05.439Z'
---

# NEM Stack 7.5 - Making the API Better

## API - Filtering

* Filter data using a query string.
  * Rather than get all tours, it would be get all tours that matches the query string.
  * `?duration=5&difficulty=easy`
* How to access this data in express:
  * `req.query` - This should give us a formatted object with `{ duration: '5', difficulty: 'easy' }`
* Writing database queries:
  * Write a query like we did with mongoDB.

```javascript

const tours = await Tour.find({
  duration: 5,
  difficulty: 'easy'
});

OR

const tours = await Tour.find(req.query);

```
 * Mongoose methods:

 ```javascript
 
 const tours = await Tour.find()
  .where('duration')
  .equals(5)
  .where('difficulty')
  .equals('easy');
 
 ```

### To Exclude Items

```javascript

const queryObj = {...req.query};
const excludedFields = ['page', 'sort', 'limit', 'fields'];
excludedFields.forEach(el => delete queryObj[el])

```

* As soon as you use await with the query, the promise will come back with the document that is from the result of the query.
  * We have to keep the `Tour.find(queryObj)` as a separate query.
  * Only use await once we've chained everything we need, we can then await it.


## Advanced Filtering

* To add greater than etc use: `/tours?duration[gte]=5&difficulty=easy`

```javascript

let queryStr = JSON.stringify(queryObj);
queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, match => `$${match}`);
const query = Tour.find(JSON.parse(queryStr));

```

## Results Sorting

* Using `tours?sort=price,ratingsAverage`:

```javascript

let query = Tour.find(JSON.parse(queryStr))

if(req.query.sort) {
  const sortBy = req.query.sort.split(',').join(' ');
  query = query.sort(sortBy)
} else {
  query = query.sort('-createdAt')
}

```
* We wanted to chain a new method (.sort) to the query.
* To sort by descending use `tours?sort=-price`
* To combine sort queries we use sort('price ratingsAverage')#


## Limiting Fields

* `/tours?fields=name,duration,difficulty,price`

```javascript

if (req.query.fields) {
  const fields = req.query.fields.split(',').join(' ');
  query = query.select(fields);
} else {
  query = query.select('-__v');
}

```
* using `-__v` in the query.select, mongoose will not select the v.
* If you want to hide a value all the time, you can do so in the schema:

```javascript

createdAt: {
  type: date,
  default: Date.now(),
  select: false
},

```

## Pagination

* `tours?page=2&limit=10`

```javascript

// req.query.page * 1 - turns a string in to a number
const page = req.query.page * 1  || 1;
const limit = req.query.limit * 1 || 100;
const skip = (page - 1) * limit;

//page=2&limit=10
//query = query.skip(10).limit(10);

query = query.skip(skip).limit(limit);

if (req.query.page) {
  const numTours = await Tour.countDocuments();
  if(skip >= numTours) throw new Error('This page does not exist');
}

```

## Aliasing

* Rather than: `/tours?limit=5&sort=-ratingsAverage,price`:

```javascript

router.route('/top-5-cheap').get(tourController.aliasTopTours, tourController.getAllTours);

exports.aliasTopTours = (req, res, next) => {
  req.query.limit = '5';
  req.query.sort ='-ratingsAverage,price';
  req.query.fields = 'name,price,ratingsAverage,summary,difficulty';
  next();
};

```

## Refactoring API Features

* Create a class for the feature:

```javascript
class APIFeatures {
  constructor(query, queryString) {
    this.query = query;
    this.queryString = queryString;
  }

  filter() {
    const queryObj = {...this.queryString};
    const excludedFields = ['page', 'sort', 'limit', 'fields'];
    excludedFields.forEach(el => delete queryObj[el])

    let queryStr = JSON.stringify(queryObj);
    queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, match => `$${match}`);

    this.query = this.query.find(JSON.parse(queryStr));

    return this;
  }
}

module.exports

const APIFeatures = require('../../utils/apiFeatures');

const features = newAPIFeatures(Tour.find(), req.query).filter().sort();
const tours = await features.query;

```

