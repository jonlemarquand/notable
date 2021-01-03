---
title: MERN Stack 5 - Building the Frontend
created: '2020-05-09T08:51:24.052Z'
modified: '2020-05-10T08:21:09.586Z'
---

# MERN Stack 5 - Building the Frontend

## Starting Setup, Pages & Routes

* Option One:
  * components
    * not full screen components
  * pages
    * full screen components
* Option Two:
  * Split the logic by feature
  * places > components + pages
  * users > components + pages
  * shared > components

### React-router

* `npm install --save react-router-dom`
  * This helps us mimic url addresses to render full screen pages.

```javascript

import React from 'react';
import { BrowserRouter as Router, Route, Redirect, Switch } from 'react-router-dom';

import Users from './user/pages/Users';
import NewPlace from './places/pages/NewPlaces';

function App() {
  return <Router>
    <Switch>
      <Route path="/" exact>
        <Users />
      </Route>
      <Route path="/places/new" exact>
        <NewPlace />
      </Route>
      <Redirect to="/" />
    </Switch>
  </Router>;
}

export default App;


```
* We wrap router around eveyrthing that should be able to use our router.
* By default "/" is treated as a default. So anything after the / will also render the page.
  * To get around this use `<Route path="/" exact>`
  * Exact also lets you use active links.
* If you use `<Redirect to="/">` this will automatically return to the default screen if there is no matching route.
  * However, this would still be exectued in the next lines.
  * We need the `<Switch>` command to wrap. (Almost like if/else statements between routes)
* To keep components lean, have very few things happening in each component.
  * E.g. delegate the rendering of a users list to a UserList component.

## Presentational vs Stateful Components

* Some components are dumb, just outputting content and structure.
  * The card component for example, just renders a div, applies some styles and renders content that it receives.
  * Normal to have more presentational components than stateful components.
* Other components manage state.

## React Transition Group

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { CSSTransition } from 'react-transition-group';

import './SideDrawer.css';

const SideDrawer = props => {
    const content = (
        <CSSTransition 
        in={props.show} timeout={200} classNames="slide-in-left" mountOnEnter unmountOnExit>
            <aside className="side-drawer">{props.children}</aside>
        </CSSTransition>
    );
    return ReactDOM.createPortal(content, document.getElementById('drawer-hook'));

};

export default SideDrawer;

```
* In this, the CSSTransition can be wrapped around a component to animate it.
* It accepts various props, including in, timeout, classNames (how it should be animated)
  * mountOnEnter and unmountOnExit tells the component to be part of the DOM on entry and remove it when it is hidden.

## useParams

* useParams is a useful feature of `react-router-dom` to pull the unique id from the URL and be able to use them.
  * i.e. to filter content `const userId = useParams().userId;` 
  * and then `const loadedPlaces = DUMMY_PLACES.filter(place => place.creator === userId);`
