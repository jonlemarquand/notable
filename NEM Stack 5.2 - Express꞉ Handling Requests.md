---
title: 'NEM Stack 5.2 - Express: Handling Requests'
created: '2020-04-07T09:13:45.689Z'
modified: '2020-04-08T07:01:34.526Z'
---

# NEM Stack 5.2 - Express: Handling Requests

## Starting Our API: Handling GET Requests

```javascript

const tours = JSON.parse(
  fs.readFileSync(`${__dirname}/dev-data/data/tours-simple.json`)
);

app.get('api/v1/tours', (req, res) => {
  res.status(200).json({
    status: 'success',
    results: tours.length,
    data: {
      tours
    }
  })
});

```
* It's a good practice to use versions with the API so old users could still be using the old one.
* This handles a request of getting all the tours.
  * The file is read from a json file, in sync outside the event loop, and then converted to an array.
  * The array is then passed to the client via the request in the event loop.

## Handling POST requests

* The URL for post and get is the same, just the HTTP method changes.
* The request, in this case, may have some data on it.
  * However, express, by default doesn't put data on the request.
  * We need ot use some middleware: `app.use(express.json());`
    * This is basically just a function that can modify the incoming request data.
    * Middleware: Stands between the request and the response.
    * The data from the body is added to the request.
* HTTP Code 201 - stands for created

```javascript

app.post('/app/v1/tours', (req, res) => {
  //console.log(req.body);

  const newID = tours[tours.length - 1].id + 1;
  const newTour = Object.assign({id: newId}, req.body);

  tours.push(newTour);
  fs.writeFile(`${__dirname}/dev-data/data/tours-simple.json`, JSON.stringify(tours), err => {
    res.status(201).json({
      status: 'success',
      data: {
        tour: newTour
      }
    });
  });
})

```
* With POST requests, you still need to send back somethign to end the request, response cycle.
* In JSON, everything has to use double quotes.
* Can only send one response.


## Responding to URL Parameters

```javascript

app.get('api/v1/tours/:id', (req, res) => {
  
  const id = req.params.id * 1;
  const tour = tours.find(el => el.id === id)

  if (id  > tours.length) {
    return res.status(404).json({
      status: 'fail',
      message: 'Invalid ID'
    });
  }

  res.status(200).json({
    status: 'success',
    data: {
      tour
    }
  })
});

```

* The `/:id` creates a variable called id.
  * It is available in `req.params`.
  * So if we call `/api/v1/tours/5` it will return an object with id: 5.
* To make a parameter optional use `/:id?`
  * This can be useful if you were to use `/:id/:x/:y?`. It would only require id and x.


## Handling Patch Requests

* Put - the entire properties
* Patch - only the updated properties

```javascript

app.patch('/api/v1/tours/:id', (req, res) => {
  if (req.params.id  > tours.length) {
    return res.status(404).json({
      status: 'fail',
      message: 'Invalid ID'
    });
  }

  res.status(200).json({
    status: 'success',
    data: {
      tour: '<Updated tour here...>'
    }
  });
});

```

## Handling Delete Requests


```javascript

app.delete('/api/v1/tours/:id', (req, res) => {
  if (req.params.id  > tours.length) {
    return res.status(404).json({
      status: 'fail',
      message: 'Invalid ID'
    });
  }

  res.status(204).json({
    status: 'success',
    data: null
  });
});

```

## Refactoring our Routes

* Route: Http method + link
* Route handler: (req, res) => {}
* Rather than each route having it's own callback, combine it to:

```javascript

const getAllTours = (req, res) => {
  res.status(200).json({
    status: 'success',
    results: tours.length,
    data: {
      tours
    }
  });
};

//app.get('/api/v1/tours', getAllTours);

app.route('/api/v1/tours')
  .get(getAllTours)
  .post(createTour);

```
* Using app.route.get.post combines them all into a neater format.

## Middleware and the Request-Response Cycle

* Request (Req Obj, Res Obj) -> Middleware ... next() -> Response
* Everything is middleware (even routers)
  * Routes are middleware that are only run in certain locations.
* All together middleware is known as middleware stack.
  * Order as defined in the code!
  * Order of the code matters a lot in express.
  * Final middleware has res.send(...) which sends the response back to the client.
* Consider it as a pipeline.

### Creating our own Middleware

* `app.use(express.json());`
  * This returns a function which is added to the middleware stack.
* To create our own:

```javascript

app.use((req, res, next) => {
  console.log('Hello from the middleware');
  next();
});

```
* Always, always, always call next. Otherwise the app will get stuck.
* Middleware will always run from each and every single request unless specified.
  * In the above example, every request will log the message to the console.
  * If the middleware is written after the app.route then it will not be run in the route request.
    * Order is very important.

### Param Middleware

* We can run middleware only when a certain url is present.
  * `router.param('id', (req, res, next, val) => { console.log(`Tour id is: ${val}`); next();})`

### Chaining Multiple Middleware Functions

* Add to the post handler stack: `.post(middleware, tourController.createTour);`

## Creating and Mounting Multiple Routers

```javascript
const tourRouter = express.Router();

tourRouter
  .route('/')
  .get(getAllTours)
  .post(createTour);

tourRouter
  .route('/:id')
  .get(getTour)
  .delete(deleteTour);

app.use('/api/v1/tours', tourRouter);

```

## A Better File Structure

* Folder > routes > userRoutes.js + tourRoutes.js
  * In each file you need to import the express module - `const express = require('express')`
  * Convention in each Routes.js file to just use `router` rather than `tourRouter`
  * You can then export that using `module.exports = router;`
  * To import use: `const tourRouter = require('./routes/tourRoutes')`
  * You then mount the routers in the index file with `app.use('api/v1/tours', tourRouter);`
* Folder > controllers > tourController.js + userController.js
  * This is the functions of what happens when the link is called.
  * `const getAllTours` would become `exports.getAllTours`
  * Import as `const tourController = require('./../controllers/tourController');` in the tourRoutes file.
  * You can then use `tourController.getAllTours`
* server.js file
  * Everything that is related to express in one file, everything related to the server in one file.
  * `module.exports = app;` - app.js
  * `const app = require('./app');` - server.js

## Serving Static Files

* `app.use(express.static(`${__dirname}/public`));

## Environment Variables

* Most important ones are development and production environments.
* By default, express sets the environment to development.
* `NODE_ENV=development nodemon server.js`
* Could define different databases, set username/password etc as environment variables.
* Create a config file. `config.env`
  * `NODE_ENV=development USER=jon PASSWORD=123456 PORT=8000`
  * How to read this file: `npm i dotenv` and then `dotenv.config({path: './config.env' });` and `const dotenv = require('dotenv')` in server.js
* There is a dotEnv extension in VSCode
* To use: `if(process.env.NODE_ENV === 'development'){app.use(morgan('dev'));}`
  * The process is available to us in every single file.

## Setting up ES Lint + Prettier in VSCode

* `npm i eslint prettier eslint-config-prettier eslint-plugin-prettier eslint-config-airbnb eslint-plugin-node eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react --save-dev`


