---
title: 'NEM Stack 9 - Authentication, Authorisation and Security'
created: '2020-04-18T09:31:27.566Z'
modified: '2020-04-22T07:08:51.304Z'
---

# NEM Stack 9 - Authentication, Authorisation and Security

## Modelling Users

* Its about letting users log in and access routes they would otherwise not have access to.

```javascript
const mongoose = require('mongoose');
const validator = require('validator');

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Please tell us your name'],
  },
  email: {
    type: String,
    required: [true, 'Please provide your email'],
    unique: true,
    lowercase: true
    validate: [validator.isEmail, 'Please provide a valid email']
  },
  photo: String,
  password: {
    type: String,
    required: [true, 'Please provide a password'],
    minlength: 8,
    select: false
  },
  passwordConfirm: {
    type: String,
    required: [true, 'Please confirm your password']
  }
});

const User = mongoose.model('User', userSchema);
module.exports = User;
```

## Creating New Users

* All the functions related to authentication go to `authController.js`

```javascript

const User = require('./../models/userModel');
const catchAsync = require

exports.signup = catchAsync(async (req, res, next) => {
  const newUser = await User.create(req.body);

  res.status(201).json({
    status: 'success',
    data: {
      user: newUser
    }
  });
});

// In userRoutes.js

router.post('/signup', authController.signup);

```

## Managing Passwords

### Password Validation

```javascript

passwordConfirm: {
  type: string,
  required: [true, 'Please confirm your password'],
  validate: {
    // THIS WILL ONLY WORK ON SAVE!!!
    validator: function(el) {
      return el === this.password;
    },
    message: 'Passwords are not the same!'
  }
}

```
* This does mean we can't use find one and update when they are updating the user details.
  * This will only work on CREATE and SAVE

### Password Encryption

* Never store passwords in plain text in a database.
* Encrypt/hashing is the same thing for passwords.
* `npm install bcryptjs` and import `const bcrypt = require('bcryptjs');`

```javascript

userSchema.pre('save', async function(next) {
  // Only run this function if the password was actually modified.
  if(!this.isModified('password')) return next();
  // Hash the password with cost of 12
  // The number is how CPU intensive the salt string should be. The better the encyrption. 10 is default, 12 is good.
  this.password = await bcrypt.hash(this.password, 12);
  // Delete the password confirm field.
  this.passwordConfirm = undefined;
  next();

})

```

## How Authentication with JWT Works

* JSON Web Tokens
  * Stateless solution.
* How it works:
  * User makes a post request to login.
  * Server checks if user and password is right, creates a unique JWT - Secret string.
  * The server then sends the JWT back to the client.
  * The user stores it in a cookie or localstorage.
    * The server doesn't know which users are logged in.
    * A bit like a passport to access different parts of the application.
  * For access, when the user goes to a protected route, he takes the JWT.
    * Like showing the passport.
    * App will then verify the JWT. If valid, allow access. If not, block the route.
* All of this has to be done over HTTPS.
* A JWT is made up of:
  * A header
  * A payload
    * Both headers and payloads are not encrypted.
  * A signature
    * Created using the header, payload and secret stored on the server.
* Verification:
  * Checks to see if the header/payload has changed. (By a third party)
  * It will create a test signature with the header/payload and secret on the server.
    * It then compares the test signature and the signature from the client.


## Signing Up Users

* Right now we create a new user with all the data being received from the body.
* To fix, replace:

```javascript

//This code
const newUser = await User.create(req.body);

// For this code
const newUser = await User.create({
  name: req.body.name,
  email: req.body.email,
  password: req.body.password,
  passwordConfirm: req.body.passwordConfirm
});

```
* In this new code, we only allow the data to use these options.

* Install `npm install jsonwebtoken`
* To auto login, once they've signed up:


```javascript
const jwt = require('jsonwebtoken');

exports.signup = catchAsync(async (req, res, next) => {
  const newUser = await User.create(req.body);

  const token = jwt.sign({ id: newUser._id }, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRES_IN
  });

  res.status(201).json({
    status: 'success',
    token,
    data: {
      user: newUser
    }
  });
});

// In the config.env file:
JWT_SECRET=32CHARACTERSATLEAST
JWT_EXPIRES_IN=90d // could also use 10h 5m 3s or any number which would be treated as ms

```

* id: newUser._id is the payload, 'secret' is the secret.
* The JWT secret should be at least 32 characters long.
  * Can just be `JWT_SECRET=my-ultra-secure-and-ultra-long-secret`

## Logging in Users

* Sign a JSON web token and send it back to the client.
  * As long as the user exists and the password is correct.

```javascript

const AppError = require('./../utils/AppError');

exports.login = catchAsync (async (req, res, next) => {
  // const email = req.body.email; - using destructuring
  const { email, password } = req.body

  // 1. Check is email and password exist
  if(!email || !password) {
    return next(new AppError('Please provide email and password!', 400));
  }
  // 2. Check if user exists && pasword is correct
  const user = await User.findOne({ email }).select('+password');
  const correct = 

  if(!user || !(await user.correctPassword(password, user.password))) {
    // Don't specify if the email or password is wrong, makes it more secure
    return next(new AppError('Incorrect email or password', 401))
  }

  // 3. If everything ok, send token to client
  const token = signToken(user._id);
  res.status(200).json({
    status: 'success',
    token
  })
});

// userModel.js

userSchema.methods.correctPassword = async function(candidatePassword, userPassword) {
  return await bcrypt.compare(candidatePassword, userPassword);
  // Will return true is the two passwords are the same.
}


```
* An instance method is a method that will be avaialble in all documents on a certain method


## Protecting Tour Routes

* What if we only wanted to allow logged in users to get all tours?
  * We'd need some sort of check in place to see whether the user is logged in or not.
  * Create a middelware function that sits between the handlers.
    * Return an error if the user is not logged in, or calls the getAllTours handler.
* THe standard for sending a token:
  'Authorization' : 'Bearer sdfnfsfsdfgsdfsgr'
* Errors:
  * JSONWebTokenError - Payload has changed, token is invalid.
    * `if (error.name === 'JsonWebTokenError') error = handleJWTError()`
    * `const handleJWTError = ()) => new AppError('Invalid token. Please log in again!', 401)` - Only in production.
  * TokenExpiredError - The token has expired.
    * `if (error.name === 'TokenExpiredError') error = handleJWTExpiredError()`
    * `const handleJWTExpiredError = ()) => new AppError('Your token has expired! Please log in again!', 401)`


```javascript

// authController.js

const { promisify } = require('util');

exports.protect = catchAsync(async (req, res, next) => {

  // 1. Getting token and check if it's there
  let token;
  if (req.headers.authorization && req.headers.authorization.startsWith('Bearer')) {
    token = req.headers.authorization.split(' ')[1];
  }
  if(!token) {
    return next(new AppError('You are not logged in! Please log in to get access.', 401));
  }

  // 2. Validate the token
  const decoded = await promisify(jwt.verify)(token, process.env.JWT_SECRET);

  // 3. Check if user still exists
  const freshUser = await User.findById(decoded.id);
  if(!freshUser) {
    return next(new AppError('The user belonging to this token no longer exists.', 401));
  }

  // 4. Check if user changed password after the token was issued.
  if(freshUser.changedPasswordAfter(decoded.iat)) {
    return next(newAppError('User recently changed password! Please log in again.', 401));
  };

  // GRANT ACCESS TO PROTECTED ROUTE
  req.user = freshUser;
  next();
});

// tourRoutes.js

const authController = require('./../controllers/authController');

router
  .route('/')
  .get(authController.protect, tourController.getAllTours)

// userModel.js

passwordChangedAt: Date

userSchema.methods.changedPasswordAfter = function(JWTTimestamp) {
  if(this.passwordChangedAt) {
    const changedTimestamp = parseInt(this.passwordChangedAt.getTime() / 1000, 10);
    return JWTTimestamp < changedTimeStamp;
  }
  // False means NOT changed
  return false;
}

```

## Advanced Postman Setup

* Environments: A context where the app is running.
  * Development and production.
  * In the dev, we can then define the URL: http://127.0.0.1
  * We can then use: `{{URL}}api/v1/tours`
    * This will come in handy when testing the production version of the app.
* Automate the token copy and pasting.
  * Tests tab > Set environment variable > pm.environment.set("jwt", "pm.response.json().token);
  * Set bearer token in get all tours. > {{jwt}}


## Authorization: User Roles and Permissions

```javascript

// tourRoutes.js

router
  .route('/:id')
  .delete(
    authController.protect,
    authController.restrictTo('admin'), 
    tourController.deleteTour
  );

// userModel.js

role: {
  type: String,
  enum: ['user', 'guide', 'lead-guide', 'admin'],
  default: 'user'
}

// authController.js

exports restrictTo = ()

```

