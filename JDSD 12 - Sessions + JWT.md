---
title: JDSD 12 - Sessions + JWT
created: '2020-05-03T08:37:43.220Z'
modified: '2020-05-05T10:18:54.419Z'
---

# JDSD 12 - Sessions + JWT

## Cookies vs Tokens

* Cookies
  * Browser sends POST request to sign in
  * Server authenticates this and returns a response with a header set-cookie: session=randompieceofstring.
  * If the browser wants to access it, it sends a request to the server.
  * The server checks the cookie and returns the user.
  * It is stateful, an authentication session must be kept in both the server and client.
    * The server has to assess if the cookie is legit.
* Token-Based Authentication
  * Most common is the JWT: The JSON Web token.
  * A user tries to log in with a POST Request
  * The server checks the info and send the token.
    * At this point, looks quite similar to a cookie.
  * The token is stored on the browser.
    * Any time the browser makes the request, they have to send the JWT.
  * The server then validates the token and can send whatever data back.
  * Stateless, the server doesn't need to keep a record of what users are logged in.
* Pros & Cons:
  * Tokens + = Each token is self-contained. The server is stateless.
  * Tokens + = Data can be inside the JWT
  * Tokens + = A token based approach makes it easy to work with different apis.
    * You can use tokens across different servers.
  * Tokens + = Both mobile platforms and sites.
  * Tokens - = JWT are a lot bigger than cookies.
  * Tokens - = If you store info, the JWT can be stolen and read the data.

## User Authentication

```javascript

const handleSignin = (db, bycrypt, req, res) => {
  const { email, password } = req.body;
  if (!email || password) {
    return Promise.reject('incorrect form submission');
  }
  return db.select('email', 'hash').from('login')
    .where('email', '=', email)
    .then(data => {
      const isValid = bcrypt.compareSync(password, data[0].hash);
      if (isValid) {
        return db.select('*').from('users')
          .where('email', '=', email)
          .then(user => user[0])
          .catch(err => Promise.reject('unable to get user'))
      } else {
        Promise.reject('wrong credentials')
      }
    })
    .catch(err => Promise.reject('wrong credentials'))
}


const getAuthTokenId = () => {
  console.log('Auth ok')
}
const signinAuthentication = (db, bcrypt) => (req, res) => {
  // check if the authorisation token already exists.
  const { authorization } = req.headers;
  return authorization ? getAuthTokenId() : 
    handleSignin(db, bcrypt, req, res)
      .then(data => res.json(data))
      .catch(err  => res.status(400).json(err))
}


```
* With Node/Express only the handler of the endpoint should be returning a response from the server.
  * Other functions are now just a helper function.


## Sending the JWT Token

```javascript

const handleSignin = (db, bycrypt, req, res) => {
  const { email, password } = req.body;
  if (!email || password) {
    return Promise.reject('incorrect form submission');
  }
  return db.select('email', 'hash').from('login')
    .where('email', '=', email)
    .then(data => {
      const isValid = bcrypt.compareSync(password, data[0].hash);
      if (isValid) {
        return db.select('*').from('users')
          .where('email', '=', email)
          .then(user => user[0])
          .catch(err => Promise.reject('unable to get user'))
      } else {
        Promise.reject('wrong credentials')
      }
    })
    .catch(err => Promise.reject('wrong credentials'))
}


const getAuthTokenId = () => {
  console.log('Auth ok')
}

const signToken = (email) => {
  const jwtPayload = { email };
  return jwt.sign(jwtPayload, 'JWT_SECRET', { expiresIn: '2 Days'});
}

const createSessions = (user) => {
  const { email, id } = user;
  const token = signToken(email);
  return {success: 'true', userId: id, token }
}

const signinAuthentication = (db, bcrypt) => (req, res) => {
  // check if the authorisation token already exists.
  const { authorization } = req.headers;
  return authorization ? getAuthTokenId() : 
    handleSignin(db, bcrypt, req, res)
      .then(data => {
        return data.id && data.email ? createSessions(data) : Promise.reject(data)
      })
      .then(session => res.json(session))
      .catch(err  => res.status(400).json(err))
}


```

* `npm install jsonwebtoken`


## Adding Redis

```javascript

const redis = require("redis")

//setup redis
const redisClient = redis.createClient(process.env.REDIS_URI);

```

### Adding Redis to Docker Compose

```yml

# Backend API

smart-brain-api:
  environment:
    REDIS_URI: redis://redis:6379
  links:
    - redis

# Redis

redis:
  image: redis
  ports:
    - "6379:6379"

```

## Storing JWT Tokens

```javascript

const signToken = (email) => {
  const jwtPayload = { email };
  return jwt.sign(jwtPayload, 'JWT_SECRET', { expiresIn: '2 Days'});
}

const setToken = (token, id) => {
  return Promise.resolve(redisClient.set(token, id))
}

const createSessions = (user) => {
  const { email, id } = user;
  const token = signToken(email);
  return setToken(token,id)
    .then(() =>{ return {success: 'true', 'userId: id, token }})
    .catch(console.log)
}

```

## Retrieving Auth Token

* We need to make sure when we receive the token, we check it.

```javascript

const getAuthTokenId = (req, res) => {
  const { authorization } = req.headers;
  return redisClient.get(authorization, (err, reply) => {
    if (err || !reply) {
      return res.status(400).json('Unauthorized');
    }
    return res.json({id: reply})
  })
}

```

## Client Session Management

* To save the token to the browser:

```javascript

saveAuthTokenInSession = (token) => {
  window.sessionStorage.setItem('token', token);
}

// in the onSubmitSignIn

.then(data => {
  if (data.userId && data.success === 'true') {
    this.saveAuthTokenInSession(data.token)
    this.props.loadUser(data)
    this.props.onRouteChange('home');
  }
})
```
* Local Storage vs Session Storage
  * Local persists a little bit longer than session storage
  * Session storage is just for one single storage.

```javascript

componentDidMount() {
  const token = window.sessionStorage.getItem('token');
  if (token) {
    fetch('http://localhost:3000/signin', {
      method: 'post',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer ' + token
      }
    })
      .then(resp => resp.json())
      .then(data => {
        if (data && data.id) {
          fetch(`http://localhost:3000/profiles/${data.id}`, {
            method: 'post',
            headers: {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer ' + token
            }
          })
          .then (resp => resp.json())
          .then (user => {
            if (user && user.email) {
              this.loadUser(user)
              this.onRouteChange('home');
            }
          })
        }
      })
      .catch(console.log(err))
  }
}

```
* In signin.js they have to sign in, and if everything checks out the database will generate a token which will be saved in the browser.
* If the user refreshes, we check the app.js componentdidmount and see if there was a token.
  * If there is a token we run the sign-in endpoint, we automatically get and fetch the authorisation token header.


## Authorisation Middleware

```javascript
//server.js
app.get('/profile/:id', auth.requireAuth, (req, res) => { profile.handleProfileGet(req, res, db) })

//authorization.js
const redisClient = require(.'/signin').redisClient;

const requireAuth = (req, res, next) => {
  
  const { authorization } = req.headers;
  if(!authorization) {
    return res.status(401).json('Unauthorised');
  }
  return redisClient.get(authorization, (err, reply) => {
    if (err || !reply) {
      return res.status(401).json('Unauthorised');
    }
  return.next();
  })
}

//app.js
headers: {
  'Content-Type': 'application/json',
  'Authorization': window.sessionStorage.getItem('token')
}
```
