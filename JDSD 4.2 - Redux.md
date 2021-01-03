---
title: JDSD 4.2 - Redux
created: '2020-03-08T11:19:50.000Z'
modified: '2020-03-27T08:16:40.751Z'
---

# Redux

* The better our ddelivery of Javascript, the faster we build the render tree, the faster our page loads.
* If we change something in the DOM we have to go through the render tree again and paint.
* Webpack - Helps optimise javascript delivery.
* Redux - Adds to React to improve the rendertree layout --> paint --> process.

## State Management

* State described what our app should look like.
    * Think of state as memory, an app needs to remember things in order to work.
    * Static webpages don't really have state, they don't know who the user is or interactions that are happening.
* State management is a really hard problem as our apps get bigger and bigger.
* Redux just props being passed down.
    * It stores the state in an object.

## Why Redux?

* Redux is:
    * Good for mananging large state.
    * Useful for sharing data between containers.
    * Predictable state management using the 3 principles.
        * Single source of truth.
            * One massive state object that describes everything within the app.
        * State is read only.
            * Prevents unexpected errors.
            * The state object doesn't get modified, but we create a new state object after every change.
        * Changes using pure functions.
            * A pure function is something that receives an input and always sends an output that is predictable.
* New Phrases:
    * Action - Something that a user does.
        * Clicking on a button etc.
    * Reducer - A pure function that receives an input and creates an output.
    * Store - Store in Redux, is the entire state of the app.
    * Make changes - React makes changes based on the store.
* Redux uses an architectural pattern called flux:
    * Action -> Dispatcher -> Store -> View
    * One way data flow.
* Redux === this.state (In React)
* One Caveat:
    * You can still put react state in components if you want.
    * It doesn't replace react state entirely.

### Installing Redux

* `npm install redux`
  * Also need `npm install react-redux`
* Redux gives you a few helpers, but it is mostly javascript.

## Redux Actions and Reducers

* In the `src` folder create a file `actions.js`:

```javascript

export const setSearchField = (text) => ({
  type: 'CHANGE_SEARCHFIELD',
  payload: text
})

```
* This action is going to take text and return an object.
  * The type is 'change_searchfield'
  * Payload is what we send on to the reducer, which is the text that the user inputs.

* In a new file, create `constants.js`:

```javascript

export const CHANGE_SEARCHFIELD = 'CHANGE_SEARCHFIELD';

```

* The Reducer
  * After the action, we also have the reducer which reads the action and spits out state.
  * Create another file: `reducers.js`:

```javascript

const initialState = {
  searchField: ''
}

export const searchRobots = (state = initialState, action={}) => {
  switch(action.type) {
    case CHANGE_SEARCHFIELD:
      return Object.assing({}, state, {searchField: action.payload});
    default: 
      return state;
  }
}

```
* Reducers get the input of the state and an action and if they care about the action then they will do something and act upon the state.
* We are returning a new state, with everything in the current state, plus whatever the searchField property is with action.payload.
  * We received an action called CHANGE_SEARCHFIELD and so we return a new object with the current state and the payload.
  * A pure function always needs to return something, so we need the default: return state;

## Redux Store and Provider

* `import { Provider, connect } from 'react-redux`
* To create a store (from the action -> reducer -> store -> makeChanges concept) use:
  * `import { createStore } from 'redux';`
  * `const store = createStore(rootReducer)`
    * In real life, we'll have many reducers and so we want to combine them to one root reducer.
* In React, we'd have to pass down this state throughout components, however, Redux lets us use provider.

```javascript

const store = createStore(searchRobots)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>, document.getElementById('root'));

```

* With the connect component, we can make the components aware of redux.
* Smart components are considered containers, dumb components are just components.
* At the very bottom of the app.js where we do export default:
  * `export default connect(mapStateToProps, mapDispatchToProps)(App);`
  * We've told app to subscribe to any state changes in the redux store. We now need to tell it what to be interested in.
```javascript
const mapStateToProps = state => {
    return {
      searchField: state.searchRobots.searchField
    }
  }

  const mapDispatchToProps = (dispatch) => {
    return {
      onSearchChange: (event) => dispatch(setSearchField(event.target.value))
      }
  }
```

## Redux Middleware

* Middleware is like a tunnel that listens for actions and something can happen to them.
* `npm install redux-logger` - `import { createLogger } from 'redux-logger';`
* `const logger = createLogger()`
* We can apply the middleware by adding `applyMiddleware` to the `redux` package.
  * In the app itself we can use `const store = createStore(searchRobots, applyMiddleware(logger));`
* This has made it so simple to reason about the app and monitor what the app is going through.

### Redux Async Actions

* `npm install redux-thunk` and `import thunkMiddleware from 'redux-thunk'`
* For example, when waiting for the robots to return there are three states:
  * Pending - Waiting for the robots to return
  * Success - The robots returned
  * Failed - The robots did not.

```javascript

export const requestRobots = (dispatch) => {
  dispatch({type: REQUEST_ROBOTS_}); // The first thing is to dispatch the pending action.
  fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
    .then(data => dispatch({ type: REQUEST_ROBOT_SUCCESS, payload: data}))
    .catch(error => dispatch({type: REQUEST_ROBOTS_FAILED, payload: error}))
}

// In the reducers

const initialStateRobots = {
  isPending: false,
  robots: [],
  error: ''
}

export const requestRobots = (state=initialStateRobots, action={}) => {
  switch(action.type) {
    case REQUEST_ROBOTS_PENDING:
      return Object.assign({}, state, { isPending: true})
    case REQUEST_ROBOTS_SUCCESS:
      return Object.assign({}, state, { robots: action.payload, isPending: false})
    case REQUEST_ROBOTS_FAILEED:
      return Object.assign({}, state, { error: action.payload, isPending: false})
    default:
      return state;
  }
}

```
* To combine multiple reducers use `rootReducer = combineReducers({ searchRobots, requestRobots})`
  * (After this has been imported {combineReducers} in Redux)
* Thunk middleware finds a function returned in an action instead of an object, it will do something with it.

## Other Bits of React + Redux

* File structure - Group everything according to component vs different folders for reducers etc
* React router
* Utility Libraries - Added functions.
  * Ramda and Lodash and both popular.
* Styling:
  * Glamorous, styled components, css modules
* Gatsby
  * Simple, text-based websites
* Next.js
  * Popular for server-side rendered apps.
* Material-UI Components/Semantic-UI library
* Redux-Saga, Reselect
* However, every time you add something ask yourself if you really need it.
  * Any library adds extra weight and content.
  * Make sure there is a justifiable reason, that helps with the development.
  * Is the extra learning curve worth it?


