---
title: 'NEM Stack 7.3 - CRUD Commands: Documents'
created: '2020-04-10T10:50:37.403Z'
modified: '2020-04-10T11:37:25.299Z'
---

# NEM Stack 7.3 - CRUD Commands: Documents

## Creating Documents

```javascript

router.post(tourController.createTour);

exports.createTour = async (req, res) => {
  try {
    // const newTour = new Tour({})
    // newTour.save()

    const newTour = await Tour.create(req.body);

    res.status(201).json({
      status: 'success',
      data: {
        tour: newTour
      }
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: err
    })
  }
};

```

* In the second situation, we call the create method, right on the model itself.
  * Call the create method on it.
  * In that function we pass the data from req.body.
  * THis method then returns the promise. We wait for that promise to return and store it in a new const.
* From the schema, if there are things that don't appear when posted, they are ignored.

## Reading Documents

```javascript

exports.getAllTours = async (req, res) => {
  try {
    const tours = await Tour.find();
  
    res.status(200).json({
        status: 'success',
        results: tours.length,
        data: {
          tours
        }
    });
  } catch(err) {
    res.status(404).json({
      status: 'fail',
      message: err
    })
  }
};


exports.getTour = async (req, res) => {
  try {
    const tour = await Tour.findById(req.params.id);
  
    res.status(200).json({
        status: 'success',
        results: tours.length,
        data: {
          tours
        }
    });
  } catch(err) {
    res.status(404).json({
      status: 'fail',
      message: err
    })
  }
};

```

* Find method will return an array of javascript objects.
  * FindOne will find only one document.
  * findById is shorthand for `Tour.findOne({_id: req.params.id})` 

## Updating Documents

```javascript

exports.updateTour = async (req, res) => {
  try {

    const tour = await Tour.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
      runValidators: true
    })

    res.status(200).json({
        status: 'success',
        data: {
          tour
        }
    });
  } catch(err) {
    res.status(404).json({
      status: 'fail',
      message: err
    })
  }
};

```

## Deleting Documents

```javascript

exports.deleteTour = async (req, res) => {
  try {

    await Tour.findByIdAndDelete(req.params.id);

    res.status(204).json({
        status: 'success',
        data: null
    });
  } catch(err) {
    res.status(404).json({
      status: 'fail',
      message: err
    })
  }
};

```

