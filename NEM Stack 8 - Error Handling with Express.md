---
title: NEM Stack 8 - Error Handling with Express
created: '2020-04-12T12:52:42.037Z'
modified: '2020-04-14T06:24:50.508Z'
---

# NEM Stack 8 - Error Handling with Express

## Debugging Node.js with ndb

* ndb - node debugger (by google)
* `npm i ndb --global` and in scripts use "debug": "ndb server.js"
  * It is done on the same port as node is running, so node would need to be ended first.
* It then launches a headless google chrome.
* Set breakpoints.
  * We can stop the code at certain times, to see what variables are, etc.
  * Once the script is then run again, we can see what is happening.

## Handling Unhandled Routes

```javascript

app.use('/api/v1/tours', tourRouter);
app.use('/api/v1/users', userRouter);
app.all('*', (req, res, next) => {
  res.status(404).json({
    status: 'fail',
    message: `Can't find '${req.originalURL}' on this server`
  })
})

```
* We put the app.all at the end of the app.js file because it runs in order, so if express hasn't directed it to other routes, it will get to this.
  * Middleware is added to the middleware stack in the order it is defined in the code.

## An Overview of Error Handling

* Operational errors
  * Problems that we can predict will happen in the future at some point, so we just need to handle them in advance.
  * Such as: invalid path accessed, invalid user input, failed to connect to server, failed to connect to database, request timeout.
  * Nothing to do with bugs in our code, but system/network/database connection errors.
  * We want to put all these errors in an error handling middleware.
    * It provides us with a separation of concerns.
* Programming Errors
  * Bugs that we developers introduce into our code. Difficult to find and handle.
  * Such as: reading properties on undefined, passing a number where an object is expected; using await without async; using req.query instead of req.body etc.

## Implementing a Global Error Handling Middleware

* By specifying 4 parameters, express automatically knows that this middleware is an error-handling middleware.
* If we pass anything into next, express will assume it is an error.
  * Express will then skip all other middleware and go straight to the error handling middleware.

```javascript

app.all('*', (req, res, next) => {
  const err = new Error(`Can't find ${req.originalUrl} on this server!`);
  err.status = 'fail';
  err.statusCode = 404;

  next(err);
})


app.use((err, req, res, next) => {
  
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error'
  
  res.status(err.statusCode).json({
    status: err.status,
    message: err.message
  });
});

```

## Better Errors and Refactoring

* Create an error class in utils folder: `appError.js`

```javascript
// appError.js

class AppError extends Error {
  constructor(message, statusCode) {
    super(message);

    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    this.isOperational = true;

    Error.captureStackTrace(this, this.constructor)
  }
}

module.exports = AppError;

// app.js

app.all('*', (req, res, next) => {
  next(new AppError(`Can't find ${req.originalUrl} on this server!`, 404));
})


```
* Every error gets a stack trace: `console.log(err.stack)`
* `Error.captureStackTrace(this, this.constructor)`
  * This function call will now not appear in the stack trace.

## Catching Errors in Async Functions

* In all async functions we have try/catch blocks. This is how we have been catching errors up to this point.
* Explanation of the code:
  * The async function is being passed as an argument to the catchAsync function. 
    * The reason for this is to move the error handling outside this async function to write DRY (Do Not Repeat Yourself) code. 
    * By doing so, because async functions always returns a promise (and as we know promises can either be resolved or rejected), then this allows us to pass that functionality to the catchAsync function. *Now, I saw Adam mention in a response that 
    * When returning a middleware function, Express passes the 3 arguments (req, res, next).
  * Given that, had we not wrapped fn inside the anonymous function we returned, then fn would be executed immediately. We don't want that. 
    * We want express to execute it when a request is made. So by wrapping it inside of the anonymous function, we're not executing it, because it's not a function call. 
    * Now, because Express is expecting a function as an argument whenever a request is made, Express will only then execute codeblock and therefore execute fn. 
    * Now, fn as we remember is an async function, so if the promise returned from fn is not resolved, then it's rejected and must be caught, so we'd append the .catch. 

```javascript

const catchAsync = fn => {
  return (req, res, next) => {
    fn(req, res, next).catch(next)
  };
};

exports.createTour = catchAsync(async (req, res, next) => {
  const newTour = await Tour.create(req.body);
  res.status(201).json({
    status: 'success',
    data: {
      tour: newTour
    }
  });
});

```

## Adding 404 Not Found Errors

```javascript

if (!tour) {
  return next(new AppError('No tour found with that ID', 404))
}

```
* 0 Records in get all tours, is not an error. It is that there are no tours matching the query.

## Errors During Development vs Production

* This stops too much information being sent to the client.

```javascript

if(process.env.NODE_ENV === 'development') {
  res.status(err.statusCode).json({
    status: err.status,
    error: err,
    message: err.message,
    stack: err.stack
  });
} else if (process.env.NODE_ENV === 'production') {
  if (err.isOperational) {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.message,
    });
  } else {
    console.error('ERROR', err);
    res.status(500).json({
      status: 'error',
      message: 'Oops! Something went wrong.'
    })
  }
}

```


## Mongoose Errors

### Invalid Database IDs

```javascript

// In errorController

const handleCastErrorDB = err => {
  const message = `Invalid ${err.path}: ${err.value}.`;
  return newAppError(message, 400);
}

// In the production

} else if (process.env.NODE_ENV === 'production') {

  let error = { ...err };
  if(error.name === 'CastError') handleCastErrorDB(error)

  sendErrorProd(error, res);
}

```

### Duplicate Database Fields


```javascript

if (error.code === 11000) error = handleDuplicateFieldsDB(error);

const handleDuplicateFieldsDB = err => {
  const value = err.errmsg.match()[0];//regular expression
  const message = `Duplicate field value: ${value}. Please use another value!`;
  return new AppError(message, 400)
}

```

### Mongoose Validation Errors

```javascript

if (error.name = hValidationError) error = handleValidationErrorDB;

const handleValidationErrorDB = err => {

  const errors = Objects.values(err.errors).map(el = > el.message)

  const message = `Invalid input data. ${errors.join('. ')}`;
  return new AppError(message, 400);
}

```

## Errors Outside Express: Unhandled Rejections

* An unhandled promise rejection indicates that there is a promise that did not have an error.
* Each time there is an unhandled rejection somewhere in the app, the process object will emit an onject called unhandled rejection.

```javascript
const server = app.listen(port, () => {});

process.on('unhandledRejection', err => {
  console.log(err.name, err.message);
  console.log('Unhandled Rejection! Shutting down...');
  server.close(() => {
    process.exit(1);
  })
});

```
* This gives the server a chance to finish its tasks and then close.

## Catching Uncaught Exceptions

* Bugs in syncronous code that is not handled.
* This code should be at the top of the code. So that the code is listening for code that is run later on.

```javascript

process.on('uncaughtException', err => {
  console.log('Uncaught Exception! Shutting down...');
  console.log(err.name, err.message);
  process.exit(1);
})

```
* Although these are used, it is a lot more ideal to handle errors as they would occur rather than relying on these error messages to pick them up.
