---
title: MySQL Bootcamp 17 - Building Our Web App
created: '2020-02-09T14:59:21.657Z'
modified: '2020-02-09T16:00:37.043Z'
---

# MySQL Bootcamp 17 - Building Our Web App

## Introducing Express

* Web development framework that helps us make stuff faster.

## NPM Init and Package.json

* When we make a new project, the best thing to do is:
  * `npm init` - By using a package.json file we can transfer node packages very quickly.
* `npm install express --save` - This will save the package to the package.json file.
* This is a lot smaller a file than transfering the node modules.

```javascript

var express = require('express');
var app = express();

app.get("/", function(req, res){
  res.send("HELLO FROM OUR WEB APP!");
});

app.listen(8080, function() {
  console.log('App listening on port 8080!');
});

```

* `app` is referring to the whole of the executed express.
* when the path/information is requested, the res (response object) is the answer going back.:
* the second component is to start the server.
* `get` is asking for information, but not sending information (post is the sending stuff)
* you can only have one `res.send` but usually you would respond with a file.

## Adding More Routes

* Add the MySQL code inside of the root route:

```javascript
app.get("/", function(req, res){
 var q = 'SELECT COUNT(*) as count FROM users';
 connection.query(q, function (error, results) {
 if (error) throw error;
 var msg = "We have " + results[0].count + " users";
 res.send(msg);
 });
});
```

## Adding EJS Templates

* EJS lets us put a variable javascript in html.
* `npm install --save ejs`
* `app.set("view engine", "ejs")` - This sets the 'view engine' to EJS
  * There are others like jade.
* `res.render('home')` to render the file
  * The process that happens behind the scenes: Looks for a views directory -> inside the directory it will be looking for home.viewengine so in this case `home.ejs`

```javascript
app.get("/", function(req,res){
  // Find count of users in DB
  var q = "SELECT COUNT(*) AS count FROM users";
  connection.query(q, function(err,results){
    if(err) throw err;
    var count = results[0].count;
    res.render("home",{data:count});
  });
});
```

## Connecting the Form

* POST route to send data from http.
* Using GET adds query to the route.
  * This isn't good for people's information.
  * Ok for search requests.


