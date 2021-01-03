---
title: NEM Stack 7.4 - Modelling the Tours
created: '2020-04-10T11:38:45.065Z'
modified: '2020-04-10T12:59:55.504Z'
---

# NEM Stack 7.4 - Modelling the Tours

```javascript

const mongoose = require('mongoose');

const tourSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'A tour must have a name'],
    unique: true
  },
  duration: {
    type: Number,
    required: [true, 'A tour must have a duration']
  }
  maxGroupSize: {
    type: Number,
    required: [true, 'A tour must have a group size']
  },
  difficulty: {
    type: String,
    required: [true, 'A tour must have a difficulty']
  },
  ratingsAverage: {
      type: Number,
      default: 4.5
  },
  ratingsQuantity: {
    type: Number,
    default: 0
  },
  price: {
    type: Number,
    required: [true, 'A tour must have a price']
  },
  priceDiscount: Number,
  summary: {
    type: String,
    trim: true,
    required: [true, 'A tour must have a description']
  },
  description: {
    type: String,
    trim: true
  },
  imageCover: {
    type: String,
    required: [true, 'A tour must have a cover image']
  },
  images: [String],
  createdAt: {
    type: Date,
    default: Date.now()
  },
  startDates: [Date]
});

```

* In `summary`, the `trim: true` cuts all the white space before or after a string.
* In `images`, we can define an array of strings using `[String]`
* In mongoose, `Date.now()` is converted into today's date from miliseconds.
* If you create a larger model, mongoDB will fill in the missing details with the defaults mentioned.


## Importing Development Data

```javascript

const fs = require('fs');
const mongoose = require('mongoose');
const dotenv = require('dotenv');
const Tour = require('./../../models/tourModel');

dotenv.config({ path: './config.env });

const DB = process.env.DATABASE.replace(
  '<PASSWORD>',
  process.env.DATABASE_PASSWORD
);

mongoose
  .connect(DB, {
    useNewUrlParser: true,
    useCreateIndex: true,
    useFindAndModify: false
  })
  .then(() => console.log('DB connection successful'));

// READ JSON FILE

const tours = JSON.parse(fs.readFileSync(`${__dirname}/tours-simple.json`, 'utf-8'));

// IMPORT DATA INTO DB

const importData = async () => {
  try {
    await Tour.create(tours);
    console.log('Data successfully loaded!')
  } catch(err) {
    console.log(err);
  }
};

// DELETE ALL DATA FROM DB

const deleteData = async () => {
  try {
    await Tour.deleteMany();
    console.log('Data successfully deleted!')
  } catch(err) {
    console.log(err);
  }
}

console.log(process.argv);

```

* Could just add `importData()` and `deleteData()` and then run the script OR...
* In the terminal: `node dev-data/data/import-dev-data.js --import`
* `if(process.argv[2] === '--import') { importData(); } else if (process.argv[2] === '--delete') { deleteData(); }`
* This will now mean we can use --import and --delete in the terminal to use these two functions.



