---
title: MySQL Bootcamp 3 - Creating Databases and Tables
created: '2020-01-29T13:57:45.077Z'
modified: '2020-01-29T19:32:00.971Z'
---

# MySQL Bootcamp 3 - Creating Databases and Tables

## Basic Commands

* `mysql-ctl start` - Starts the database
* `mysql-ctl stop` - Stops the database
* `mysql-ctl cli` - Starts command line interface
* `exit` - Returns you to the bash terminal
* `ctrl + c` - The keyboard exit which is a bit 'harsher'

## How to Create a Database

* Use `show databases;` to show the current databases that exist in the mysql server.
* `CREATE DATABASE name;` - to create a new database
  * i.e. `CREATE DATABASE dog_app;`
  * The most important thing is consistency to avoid any confusion.
* A note on capitalisation: You don't have to use them, but they are useful to signify what comes from SQL and what is a custom name.

## How to Delete a Database

* `DROP DATABASE <name>;`
* Drop is the command to delete something, and can also be used for tables etc.

## Using Databases

* `USE <database name>;`
* This is useful if you have different databases on the server and need some data from one of them.
* `SELECT database();` - This will tell you what database you are currently in.

## Introducing Tables

* A database is just a bunch of tables.
  * In a relational database, at least
* When we are talking about tables, and mention headers we mean the columns.
* The rows in the table, are the actual data.
* Databases are made up of lots of tables.

### The Basic Datatypes

* When you create a new table, you have to specify the datatypes of headers.
  * So you can't have 1, 3, ten, i am young
  * There are a lot of different MySQL data types.
* For now, let's work with: INT (numeric) and VARCHAR (text strings)

#### INT

* Used for whole numbers
* Up to: 4294967295
* INT's can be both negative and 0

#### VARCHAR

* A Variable-Length String
* Char is a fixed length but VARCHAR can be between 1 and 255 characters.
* varchar(100) - this would set a maximum length of 100 characters.
  * If you go over this, all that would be stored is the first 100 characters.

## Creating Tables

```SQL

CREATE TABLE tablename

  (
    column_name data_type,
    column_name data_type
  );

```
* Good practice is to pluralise table names. 'Payments' instead of 'Payment'.

### How do you know it worked?

* `SHOW TABLES;`
  * It shows which tables exist
* `SHOW COLUMNS FROM cats;`
  * This will show more data about the table.
  * In this context you can also use `DESC cats;`
    * Not identical, but do the same thing in this context.

### Deleting Tables

* `DROP TABLE <tablename>;`


