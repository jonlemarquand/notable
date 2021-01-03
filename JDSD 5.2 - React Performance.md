---
title: JDSD 5.2 - React Performance
created: '2020-04-15T06:32:36.065Z'
modified: '2020-04-15T07:49:56.012Z'
---

# JDSD 5.2 - React Performance

* localhost:300/?react_perf
  * When recorded in the developer tools, this will let us analyse our components
  * This isn't the exact same as the produciton build of react, but it is a good indicator.
* Whenever a component is mounting/updating, it is hogging the thread. (Because javascript is single-threaded).
* If something updates in the app, it will pass the update down to all the child elements.
* By using redux, we can be smart about what compoenents update.
* Chrome Plugin: React Developer Tools
  * Can see elements, props, children when using /?react-perf.
  * You can see changes when they are made.
  * You can also tick 'highlight updates' and it will show in the code what is being rerendered.
    * The colour represents how fast it's being rerendered. Red means more often, blue means colder.
* At the moment, the header (for example) rerenders on every change:

```javascript

// new Header.js component

import React, { Component } from 'react';

class Header extends Component {
  shouldComponentUpdate(nextProps, nextState) {
    return false;
  }
  render() {
    return <h1 className='f1'>RoboFriends</h1>
  }
}

export default Header;

// App.js

import Header from '../compnents/Header';
<Header />

```
* This `shouldComponentUpdate` is a React middleware that gives us an ability to control the update cycle.
  * However, it should be used cautiously.
* Reminder, state updates are async, so rather than use:

```javascript

updateCount = () => {
  this.setState({ count: this.state.count+ 1})
}

// We should use:

updateCount = () => {
  this.setState(state => { 
    return { count: state.count + 1 }
  })
}

```
On the `shouldComponentUpdate`:

```javascript

shouldComponentUpdate(nextProps, nextState) {
  if (this.state.count !== nextState.count) {
    return true
  }
  return false
}

```

* We can use `PuerComponent` which will only change if any of the props change.
  * This does the same thing as shouldComponentUpdate, but automatically.
  * You have more control with `shouldComponentUpdate`.
  * Use both cautiously, see if it actually renders better with it.
* Why did you update. - Great NPM package for checking.
