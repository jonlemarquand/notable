---
title: JDSD 5.1 - Code Splitting
created: '2020-03-27T12:01:02.031Z'
modified: '2020-04-23T17:39:10.096Z'
---

# JDSD 5.1 - Code Splitting

## Optimising Code

* Animations are resource heavy. Use them sparcely.
* If you include inline javascript in your HTML, this is a render blocking operation.
* The holy grail:
  * A fast time ot first meaningul paint.
  * A fast time to interactive.

## Code Splitting

* How to deliver javascript files in the most efficient way possible:
  * Javascript benefits from being in small chunks to stop blocking the main thread.
  * Potentially once the homepage is launched, it can load up the other javascript files for other pages.
  * Make sure you are using the prodcution version of codebases.

### Code Splitting Part 1

* At the moment, bundle.js will load all the files in:

```javascript

render() {
  if(this.state.route === 'page1') {
    return <Page1 onRouteChange={this.onRouteChange}/>
  } else if(this.state.route === 'page2') {
    return <Page2 onRouteChange={this.onRouteChange}/>
  } else if(this.state.route === 'page3') {
    return <Page3 onRouteChange={this.onRouteChange}/>
  }
}

```

### Code Splitting Part 2

* With webpack, if we put the `import Page3 from './components/page3'` somewhere else in the code other than at the top, it will detect that and only load it.

```javascript

// in this.state

this.state = {
  route: 'page 1',
  component: ''
}

// onRouteChange

onRouteChange = (route) => {
  // No Code Splitting:
  //this.setState({route: route});
  // With Code Splitting:
  if (route === 'page1') {
    this.setState({ route: route })
  } else if (route === 'page2') {
    import('./components/Page2').then((Page2) => {
      this.setState({ route: route, component: Page2.default})
    })
  } else if (route === 'page3') {
    import('./components/Page3').then((Page3) => {
      this.setState({ route: route, component: Page3.default})
    })
  }
}

render() {
  if(this.state.route === 'page1') {
    return <Page1 onRouteChange={this.onRouteChange}/>
  } else {
    return <this.state.component onRouteChange={this.onRouteChange} />
  }
}

```

### Code Splitting Part 3

* Using an async component to wrap around things.


```javascript
//Async component - A higher order component (A component that returns another component)

import React, { Component } from 'react';

export default function asyncComponent(importComponent) {
  class AsyncComponent extends Component {
    constructor(props) {
      super(props);
      this.state = {
        component: null
      }
    }

    async componentDidMount() {
      const { default: component } = await importComponent
      this.setState({
        component
      })
    }

  render() {
    const Component = this.state.component;
    return Component ? <Component {...this.props}> : null;
  }
  }
  return AsyncComponent
}

// App.js

import AsyncComponent from './components/Asynccomponent';

render() {
  if(this.state.route === 'page1') {
    return <Page1 onRouteChange={this.onRouteChange}/>
  } else if(this.state.route === 'page2') {
    const AsyncPage2 = AsyncComponent(()=> import('./components/Page2'));
    return <AsyncPage2 onRouteChange={this.onRouteChange}/>
  } else if(this.state.route === 'page3') {
    const AsyncPage3 = AsyncComponent(()=> import('./components/Page3'));
    return <AsyncPage3 onRouteChange={this.onRouteChange}/>
  }
}

```

### Code Splitting Part 4

* There is also component based splitting.
* https://reactjs.org/docs/code-splitting.html
