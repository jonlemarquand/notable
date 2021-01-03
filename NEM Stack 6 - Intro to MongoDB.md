---
title: NEM Stack 6 - Intro to MongoDB
created: '2020-04-08T17:10:41.436Z'
modified: '2020-04-08T18:01:04.157Z'
---

# NEM Stack 6 - Intro to MongoDB

## What is MongoDB?

* A NoSQL database.
  * A collection (tables), that contains documents (rows)
  * Blog (collection) -> Rows (post)
* Key Features
  * Document based: MongoDB stores data in documents (field-value pair data structures, NoSQL)
  * Scalable: Very easy to distribute data across multiple machines as your users and amount of data grows;
  * Flexible: No document data schema required, so each document can have different number and type of fields
  * Performant
  * Free and open source, published under the SSPL license.
* Documents, Bson and Embedding
  * BSON: Data format MongoDB uses for data storage.
    * Like JSON, but typed. (Number, truthy/falsy)
  * Embedding/Denormalising: Including related data into a single document.
    * This allows for quicker access and easier data models (not always the best solution)
  * Maximum size: 16MB
  * Each document has a unique id.

## Installing MongoDB on MacOS

* Download MongoDB community server.
* `sudo cp (bin files) /usr/local/bin` to install the downloaded files.
* `sudo mkdir /data/db`
* `sudo chown -R `id -un` /data/db`
* We can now call `mongod` in the terminal.
* In a new shell write `mongo`
* `db` will then return the test database.

## Creating a Local Database

* `mongo` opens the mongo shell.
* `use name-of-db` - will switch to the database or create it if it doesn't exist.
* Specify the collection before you insert a document:
  * `db.collection.insertOne({ name: "The Forest Hiker", price: 297, rating: 4.7})`
  * `db.collection.insertMany({ name: "The Forest Hiker", price: 297, rating: 4.7})`
  * Where collection is the name of the collection.
* To check:
  * `db.collection.find()`
  * `show collections` - Will show the list of collections
  * `show dbs` - to show the databases (and size)
* To quit mongo shell: `quit()`

## CRUD Commands

### CRUD: Creating Documents

* `db.tours.insertMany([{ name: "The Sea Explorer", price: 497, rating: 4.8}, { name: "The Snow Adventure, price: 997, rating: 4.9, difficulty: "easy"}])`
* This passes in an array of two documents.

### CRUD: Querying (Reading) Documents

* `db.tours.find()` - This will return all the documents that are in a certain collection.
* To search specifically:
  * `db.tours.find({ name: "The Forest Hiker" })` - This will only return the results with this name.
  * `db.tours.find({ price: {$lte: 500} })`
    * $ is a mongo operator.
    * lte - this stands for less than or equal. lt - less than
    * This will return results where the price is less than 500.
* `db.tours.find({ price: {$lt: 500}, rating: {$gte: 4.8} })`
  * To do an AND query, specify two fields in the filter object.
* OR Query:
  * `db.tours.find({ $or: [price: {$lt: 500}, rating: {$gte: 4.8} ] })`
  * Start with the OR operator which accepts a range of conditions.
* To get just the name of the document use:
  * `db.tours.find({ price: {$lt: 500}, rating: {$gte: 4.8} }, {name: 1})`

### CRUD: Updating Documents

* `db.tours.updateOne({ name: "The Snow Adventurer"}, { $set: {price: 597} })`
  * Only the first query would have updated with `updateOne`, to update multiple queries use `updateMany`.
  * Set is another operator.
* Create new properties:
  * `db.tours.updateMany({ name: "The Snow Adventurer"}, { $set: {premium: true} })`
* `db.tour.replaceOne()` will replace a document entirely.

### CRUD: Deleting Documents

* `db.tours.deleteMany({ rating: {$lt: 4.8} })` - This will delete all documents with a rating less than 4.8.
* `db.tours.deleteMany({})` - This will delete all the documents in a collection.
* You can't come back from deleting, so be careful.

### Using Compass App for CRUD Operations

* Download Compass from Mongo.

## Hosted Databases

### MongoDB Atlas

* Create a free account.

### Connecting to our Hosted Database

* Add IP address of our mac on mongoDB Atlas
* Connect in Compass or terminal


  
