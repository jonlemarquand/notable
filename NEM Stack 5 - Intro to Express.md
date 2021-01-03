---
title: NEM Stack 5 - Intro to Express
created: '2020-04-04T16:17:16.427Z'
modified: '2020-04-04T17:04:00.874Z'
---

# NEM Stack 5 - Intro to Express

## What is Express?

* Express is a minimal node.js framework.
* It contains a very robus set of features such as: Complex routing, easier handling of requests and responses, middleware, server-side rendering.
* Express allows for rapid development of node.js applications: we don't have to reinvent the wheel
* Express makes it easier to organise our application into the MVC architecture.

## Installing Postman

* Postman is a useful tool that allows us to do API testing.
* You can enter a URL and do a request (Get/Post etc)
* Separate app you'll need to download.

## Setting Up Express and Basic Routing

* Usual step 1 is to create the package.json file: `npm init` and fill in the details
* `npm i express`
* `app.js` is the genreal convention for storing express settings.

```javascript

const express = require('express');

const app = express();

app.get('/', (req, res) => {
  res.status(200).json({message: 'Hello from the server side!', app: 'Natours');
});



const port = 3000;
app.listen(port, () => {
  console.log(`App running on port ${port}...`);
});

```
* Express apps are similar to node.js in how they run, because they are built on node.

## APIs and Restful API Design

* API: An Application Programming Interface: a piece of software that can be used by another piece of software, in order to allow applications to talk to each other.
  * The application in API can be other things, as long as it is relatively stand alone.

### The Rest Architecture

#### Separate API into logical resources
* An object or representation of something, which has data associated to it.
* Any information that can be named can be a resource
* Tours, users, reviews

#### Expose structured, resource-based URLs
* Endpoints should contain only resources (nouns) and not HTTP methods that can be performed on them.

#### Use right http methods
* `/getTour` -> Should be called `/tours` called by the GET method.
* Convention is to also use plurals
* `/tours/7` would be the tour id with 7.
* GET to read, POST to create. The endpoint name would be the same but the method is different.
* To update information use `PUT` (update the entire object) or `PATCH` (the part of the object that has changed)
* To delete use `DELETE`
* `/getToursByUser` would become `GET /users/3/tours`

#### Send data as JSON (usually)
* Very lightweight data interchange format.
* Easy for both humans and computers to read.
* All the keys have to be strings, values can be various formats.
* JSend status, and then data into an additional object.

#### Must be stateless
* All state is handled on the client.
  * This means that each request must contain all the information necessary to process a certain request.
  * The server should not have to remember previous requests.
* For example:
  * currentPage = 5
  * If we used `GET /tours/nextPage` the server would have to know the current page (i.e. from the state).
  * Instead we should use `GET /tours/page/6` - on the client side we would send this.




