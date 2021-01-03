---
title: MySQL Bootcamp 16 - Introducing Node
created: '2020-02-08T09:42:51.210Z'
modified: '2020-02-09T11:14:37.300Z'
---

# MySQL Bootcamp 16 - Introducing Node

## MySQL and Other Languages

* MySQL doesn't have to be a means to just using other programmes BUT it is really useful.
* MySQL can be used with a lot of languages: PHP, Node, Ruby, Java, Python, etc.
* Client's Computer --> NodeJS --> MySQL DB

## 5 Minutes of Node

* The basics:
  * Write some code: `console.log('Hello World');`
  * Execute the file: `node filename.js`
* Create a loop:

```javascript
for(var i= 0; i < 500; i++) {
  console.log("Hello World!");
}
```

* Run the loop, it will run 500 times.
* You could therefore, say, insert 500 users very quickly.

### Javascript and Node

* Javascript does code and exists on the client side. It can do something to the webpage on their computer.
* Node.js - An implementation of javascript written to use it on the backend.
  * Uses the exact same syntax.

## Introduction to NPM and Faker

* Node comes with thousands of libraries/modules that other people have written. These are known as packages.
* NPM - node package manager.
  * `npm install faker` - To install a package
* Faker - A node package that has loads of fake data that can be generated.
* To use Faker in a js file - `var faker = require('faker');`
  * Now you can use `faker.internet.email()` or `faker.date.past()`

## Connecting Node to MySQL

* Install the package: `npm install mysql`
* In the js file: `var mysql = require('mysql')`

```javascript

var mysql = require('mysql');

var connection = mysql.createConnection({
  host: 'localhost',
  user: 'some_username',
  database: 'some_database'
});
```

Step 2: Run Queries

```javascript

var q = 'SELECT CURDATE()';

connection.query(q, function (error, results, fields) {
   if (error) throw error;
   console.log('The solution is: ', results[0].solution);
});

connection.end();

```

* connection.query - query the 'connection' (link to MySQL databate)
* with q - Current date query.
* function(error, results, fields) - When this query is finished run all of this code.
* if error or results
* need to end the connection using `connection.end();`
* to select an answer use: `'SELECT CURTIME() as time,` and `console.log(results[0].time)`
  * This will give us the answer specifically from the first result in the array.time.
* Node libraries have their own documentation, including MySQL package.

## Selecting Using Node

```javascript

var q = 'SELECT * FROM USERS';

connection.query(q, function (error, results, fields) {
   if (error) throw error;
   console.log(results);
});

connection.end();

```

## Inserting Using Node

```javascript

var person = {
    email: faker.internet.email(),
    created_at: faker.date.past()
};
 
var end_result = connection.query('INSERT INTO users SET ?', person, function(err, result) {
  if (err) throw err;
  console.log(result);
});

connection.end();

```

* This transforms the `var person` data into the insert data commands for MySQL.

## Bulk Inserting Users

```javascript
var mysql = require('mysql');
var faker = require('faker');
 
 
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'learnwithcolt',
  database : 'join_us'
});
 
 
var data = [];
for(var i = 0; i < 500; i++){
    data.push([
        faker.internet.email(),
        faker.date.past()
    ]);
}
 
 
var q = 'INSERT INTO users (email, created_at) VALUES ?';
 
connection.query(q, [data], function(err, result) {
  console.log(err);
  console.log(result);
});
 
connection.end();
```


