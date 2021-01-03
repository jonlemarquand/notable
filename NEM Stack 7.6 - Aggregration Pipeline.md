---
title: NEM Stack 7.6 - Aggregration Pipeline
created: '2020-04-12T07:04:36.451Z'
modified: '2020-04-12T07:37:57.716Z'
---

# NEM Stack 7.6 - Aggregration Pipeline

## Matching and Grouping

```javascript

exports.getTourStats = async (req, res) => {
  try {
    const stats = await Tour.aggregate([
      {
        $match: { ratingsAverage: { $gte: 4.5} }
      },
      {
        $group: {
          _id: null,
          num: { $sum: 1 },
          numRatings: { $sum: '$ratingsQuantity'},
          avgRating: { $avg: '$ratingsAverage' },
          avgPrice: { $avg: '$price' },
          minPrice: { $min: '$price' },
          maxPrice: { $max: '$price' }
        }
      },
      {
        $sort: { avgPrice: 1 },
      }
    ]);
    res.status(200).json({
      status: 'success',
      data: {
        stats
      }
    });
  } catch(err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};

router.route('/tour-stats').get(tourController.getTourStats);

```

* These functions let us group and perform different aggregate functions on our data.
  * Stuff like minimum, maximum, average
* For `numTours`, 1 will be added to the amount for each of the documents that are passed through this promise.
* To `$sort`, use the new names and 1 for ascending, -1 for descending.
* Use `$ne:` for not equal. I.e. excluding one of the results.
* You can match multiple times during the pipeline.


## Unwinding and Projecting

```javascript

exports.getTourStats = async (req, res) => {
  try {
    const year = req.params.year * 1;
    const plan = await Tour.aggregate([
      {
        $unwind: '$startDates'
      },
      {
        $match: {
          startDates: { $gte: new Date(`${year}-01-01`), $lte: new Date(`${year}-12-31`)}
        }
      },
      {
        $group: {
          _id: { $month: '$startDates' },
          numTourStarts: { $sum: 1 },
          tours: { $push: '$name' }
        }
      },
      {
        $addFields: { month: '$_id' }
      },
      {
        $project: {
          _id: 0
        }
      },
      {
        $sort: { numTourStarts: -1 }
      },
      {
        $limit: 6
      }
    ]);
    res.status(200).json({
      status: 'success',
      data: {
        plan
      }
    });
  } catch(err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};

router.route('/tour-stats').get(tourController.getTourStats);

```
* The unwind will deconstruct an array field from the document and output one document for each element of the array.
  * We want an element for each of the tour starts in the array.
