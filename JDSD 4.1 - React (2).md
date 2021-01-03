---
title: JDSD 4.1 - React
created: '2020-02-22T09:33:38.708Z'
modified: '2020-04-24T07:01:54.825Z'
---

# JDSD 4.1 - React

* React + Redux help us build scalable apps with less bugs.

## Angular vs React vs Vue

* All of them try and manage a lot of javascript.
    * They are just tools, and like tools they are pefect for their own usecase.
* Angular
    * A framework, an entire kitchen with the same tools.
* React
    * Flexible and can evolve.
    * React is the oven, we might need other tools but this is the core element.
* Vue
    * Friednly, easier to pick up if you have a lot of junior developers.
    * Vue is the microwave

## Introduction to React.js

* jQuery was very imperative, but it was hard to scale because of dom manipulation.
* React was created at Facebook and lets you scale apps.
    * Like a bread machine: You throw in the ingredients and it makes a website with magic.
    * Can be used with mobile and vr apps with React.
        * Facebook and Netflix both use it.
* Thinking in Components
    * Everyone builds a lego block and you combine them to make the website.
    * Atoms: Icons and indivisible items
    * Molecules: Combined atoms to add functionality
    * Organisms: Little bit bigger
    * Template: Give you an idea of how it should look.
    * Page
    * Because of the way React works, you can download whole components and simply place them on your site.
        * React reduces dependencies on other elements of the website.
* One Way Data Flow
    * Data flows from top to bottom, never the other way around.
    * If a component changes, only the children know about it, never the parents.
        * Based on this, they will change accordingly.
* Virtual DOM
    * We used to be the painters, telling everything to happen.
    * With React, we give a javascript object that has the state of the website and React builds the website in the most efficient way based on this.

## create-react-app

* Contains webpack, babel, linting and debugging.
* `create-react-app foldername`
* `npm start`
    * This starts up the server on localhost.
* Instead of having to stall all our own, react scripts install the latest apps.
    * The package is also created by the developers at Facebook.
*  `ReactDOM.render(<App />), document.getElementById('root'));`
    * This is a react component, and we want to render this on the screen.
    * It automatically compiles when it is saved and you don't have to refresh the page.
    * Might need to change to Babel (javascript) to get the syntax working properly.
* To update the app:
    * In the package.json, just change the version of react scripts and then run `npm install`
* New way on install/use:

```
npx create-react-app my-app
cd my-app
npm start

```

## Your First React Component

* React has Webpack under the hood so does bundling automatically.
* React is a view library, the little robot that does the DOM manipulation.
    * ReactDOM is a different library that is used for websites. We could use ReactNATIVE or other.
* `ReactDOM.render(<App />), document.getElementById('root'));`
    * I want the ReactDOM package to perform .render (whatever this is), to (root)
    * If there's no type after it, it assumes its js.
* A component has to render something.
    * The standard is to capitalise the component name.

```
import React, { Component } from 'react';
import './Hello.css';

class Hello extends Component {
    render() {
        return <h1>Hello World</h1>
    }
}

export default App;

```

* We need to export the component at the end.
    * If you use default it will only export the one thing, everything.
    * Return expects a single line, so if we need more lines use ( around the expression )
* tachyons
    * npm package that has predetermined css.
    * a bit like bootstrap.
    * e.g. <div className='f1 tc'>
        * class is a reserved keyword name in javascript so we need to use className.
* React uses JSX which is HTML-like syntax
* Within the component you can also add props (properties)
    * e.g. <Hello greeting={'Hello' + 'React Ninja'}/>
    * In the Hello.js file we can now use {this.props.greeting} which will show the greeting.
        * This will come in handy later on when we can use live data.

## Building a React App


### Creating a Basic Component

```javascript
import React from 'react'; //Import react

const Card = () => {
    return (
        <div>
            <img alt='robots' src='' />
            <div>
                <h2> Jane Doe</h2>
                <p>jane.doe@gmail.com</p>
            </div>
        </div>
    ); // This function returns the HTML info
}

export default Card;
```

* React must be used when we are using JSX otherwise our programme won't understand it.
* If you had for example:
    * <h1>Hello World</h1><div><p>Test</p></div>
    * This would fail to compile when using `export default card;` because it contains more than one element. The div element for now.
* To render more than one of an item, it would need to be wrapped in a div, like so:

```
ReactDOM.render(
    <div>
        <card />
        <card />
        <card />
    </div>
, document.getElementById('root'));
```

### Using Props

* To export more than one object from a file, you have to destructure it using `import { robots } from './robots';
    * This says what variable to grab.
* This allows us to use the info in robots (name, email and id) in the Card:

```javascript

//In index.js
<Card id={robots[0].id} name={robots[0].name} email={robots[0].email} />

//In card.js
const Card = (props) => {

    return (
        <div className='tc bg-light-green dib br3 pa3 ma2 grow bw2 shadow-5'>
            <img src={`https://robohash.org/${props.id}?200x200`} alt='robots'/>
            <div>
                <h2>{props.name}</h2>
                <p>{props.email}</p>
            </div>
        </div>
    );
}

```

* When  working with React, the VirtualDOM keeps track of components by using a KeyProp. This can be as simple as i in a loop.
    * However, a better key in this case would be something unique like an id.
    * This means that the VirtualDOM, if an element is changed, will know what to change.

### State

* Up until now, we've worked with properties, which are passed down but we've never changed them.
* React reads the props it receives and renders something.
    * This one way data flow is nice becasue the card list is a pure function.
    * It receives an input and always returns the same output.
    * These components are known as pure or dumb components.
        * They don't need to know anything apart from they receive something and they return something.
    * Props are always just inputs we get and haven't modified them.
* STATE - The description of your app.
    * An object that describes your application.
    * State is able to change.
    * Props are things that come out of state.
        * A parent feeds state into a child component and as soon as it has received that, the child cannot change it.

```javascript

import React, { Component } from 'react';
import CardList from './cardlist';
import { robots } from './robots';
import SearchBox from './SearchBox';

class App extends Component {
    constructor() {
        super()
        this.state = {
            robots: robots,
            searchfield: ''
        }
    }
    render() {
        return (
            <div className='tc'>
                <h1>RoboFriends</h1>
                <SearchBox />
                <CardList robots={robots}/>
            </div>
        )
    }
}

export default App;

```

* State generally exists in the parent component so it can be passed down to the child components.
* Any component that uses state uses `class App`
* React uses state to render as props that can be passed down to child elements.

### Using APIs to get Info

* LifeCycle hooks: If we run one of these they will automatically trigger when the site gets loaded.
    * When you load the site the component gets 'mounted' to the document with the id of root. We're replacing the div id=root with the app.
    * With the lifestyle hooks they run through the order:
        * constructor
        * componentwillMount
        * render
        * componentdidMount
    * Updates: Something happens when the react component has a state or prop change.
    * Unmounting:
* Use fetch:

```javascript

    componentDidMount() {
        fetch('https://jsonplaceholder.typicode.com/users')
            .then(response => {
                return response.json();
            })
            .then (users => {
                this.setState({robots: users});
            });
    }

```

### Use Children to render

* Using props.children we can wrap custom components:

```javascript

const Scroll = (props) => {
    return (
        <div style={{overflowY: 'scroll', border: '1px solid black', height: '800px'}}>
            {props.children}
        </div>
    )
}

```

### Folder Structure

* Strong organisation can involve a folder structure of:
    * Containers: Things that have state
    * Components: Pure functions

### Updating NPM

* npm update will update our react scripts.
    * if we use ^ before the version, it will only install the minor update.
* npm audit and npm audit fix will also check the vulnerabilities.
* the easiest way to keep them up to date is to update the package.json

### Error Boundaries

* For the user, we want a component breaking to not ruin the app.
* In React 16 they've introduced `componentDidCatch` which runs in the lifecycle if something doesn't load.
    * In production, they will see the ErrorBoundry, however, it will still show the error in development mode (before the build is run)

```javascript

import React, { Component } from 'react';

class ErrorBoundry extends Component {
    constructor(props) {
        super(props);
        this.state = {
            hasError: false
        }
    }
    componentDidCatch(error, info) {
        this.setState({ hasError: true})
    }
    render () {
        if (this.state.hasError) {
            return <h1>Ooooooops. That is not good</h1>
        }
        return this.props.children
    }
}

```


### Quick Note: Class vs Functional App.js

* App() is now a function rather than a class.
    * 
