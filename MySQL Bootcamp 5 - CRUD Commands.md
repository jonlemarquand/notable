---
title: MySQL Bootcamp 5 - CRUD Commands
created: '2020-01-30T06:54:50.649Z'
modified: '2020-01-30T07:24:11.789Z'
---

# MySQL Bootcamp 5 - CRUD Commands

## The Basics of CRUD

* CRUD - Create, Read, Update, Delete
* These are the four main operations we perform on our data.
* Create - How do we add data
* Read - How do we find/search data
* Update - How do we change existing data
* Delete - How do we get rid of data

## Introduction to SELECT

* `SELECT * FROM cats;`
  * In this example, the `*` means "Select all columns from this table/everything we have"
* SELECT expression
  * What columns do you wnat?
  * `SELECT name FROM cats;` - Will just show us the names of cats
* `SELECT name, age FROM cats;`
  * This will select multiple columns, separated by a comma
  * This will also select the columns, based on the order they are named.

## Introduction to WHERE

* Most of the time you will be trying to specify data rather than just viewing a column.
* It allows you to get, for example, 'All cats between the age of 3-4, with the breed Tabby'
* Syntax - `SELECT * FROM cats WHERE age=4;` or `SELECT * FROM cats WHERE name='Egg';`
* By default WHERE is case insensitive

## Introduction to Aliases

* We can specify how our data should be displayed back to us.
* `SELECT cat_id AS id, name FROM cats;`

## The UPDATE Command

`UPDATE cats SET breed='Shorthair' WHERE breed='Tabby';`

* This line will find in the cats table, all the cats that have the breed tabby and then set them as shorthair.
* When you're updating something you should run the equivalent SELECT before updating data to make sure you are targeting what you mean to target.
  * There is no auto-rollback.

## Introduction to DELETE

`DELETE FROM cats WHERE name='Egg';`

* Once again, check the selection before using delete.
* IDs would not change in this instance, '4' would just be gone.
