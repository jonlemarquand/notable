---
title: MERN Stack 4 - React.js - A Refresher
created: '2020-05-08T11:54:58.124Z'
modified: '2020-05-08T15:05:47.345Z'
---

# MERN Stack 4 - React.js - A Refresher

* A Javascript library for browser-side javascript code.
* React does not run on the server or interactive with databases.
* Declarative approach: You define the result, not the steps that lead to the result.
  * You define components and build your UI with these components.
  * Every component defines what it should render under what circumstances.

## Setting Up a Starting Project

* With React, you will quite often just build a single-page application.
  * One HTML file that includes script imports which host and run the react application.
  * Because javascript interacts with the browser, we don't have to wait for a page reload.

## Understanding JSX

* In `index.js` we're importing the `<App />` component.
* In the `app.js` file, we have jsx syntax which looks a bit like HTML which will be translated by React.
  * It's the equivalent of writing, `return React.createElement('h1', {}, 'Hi, this is React!');`
  * This is also why you have to `import React from 'react';` on every page.

## Understanding Components

* A React component can be one of two things: 
  * A function that returns jsx or returns create react element calls
  * Or it can be a javascript class that returns a render method.
  * Some technical differences. In modern react you mainly use functional components.
* If you return an object or other javascript, it would not be treated as a component and would fail to render.
* The component should start with a capital letter to let React know it's a custom component.

### Working with Multiple Components

* Typically we split our app into multiple components.
* A list would be it's own component:
  * components > GoalList.js:

```javascript
// GoalList.js

import React from 'react';

const GoalList = () => {
  return (
    <ul className="goal-list">
      <li>One Goal</li>
      <li>Two Goals</li>
    </ul>
  );
};

export default GoalList;

// App.js
import GoalList from './components/GoalList'
<GoalList />

```

### Using Props to pass Data between Components

* Every list item, for example, could be their own components.
* The idea is to have lean files, which can be managed easier.
  * Keeping files small, focussed and manageable.
* Props are like attributes that we can define on components.

```javascript
// GoalList.js

import React from 'react';

const GoalList = props => {
  return (
    <ul className="goal-list">
      {props.goals.map((goal) => {
        return <li>{goal.text}</li>;
      })}
    </ul>
  );
};

export default GoalList;

// App.js
import GoalList from './components/GoalList'
const App = () => {
  const courseGoals = [
    {id: 'cg1', text: 'One Goal'},
    {id: 'cg2', text: 'Two Goals'},
    {id: 'cg3', text: 'Three Goals'},
  ];

  return (
    <div className="course-goals">
      <h2>Course Goals</h2>
      <GoalList goals={courseGoals}/>
    </div>
  );
};

```
* The curly brackets let us use javascript in jsx.
* Every function that is turned into a React component receives props.
  * This object bundles all the props that are passed to the component.
* The GoalList breakdown:
  * We map the array of plain javascript objects to an array of jsx elements which is renderable.
* Whenever you output lists of data with .map, you need a key prop:
  `return <li key={goal.id}>{goal.text}</li>`

### Handling Events

* Convention is to name props that pass functions down elements as event handlers: i.e. onAddGoal

```javascript
import React from 'react';

const NewGoal = () => {

  const addGoalHandler = event => {
    event.preventDefault();

    const newGoal = {
      id: Math.random().toString(),
      text: 'My new goal'
    };

  props.onAddGoal(newGoal);

  };

  return (
    <form onSubmit=>{addGoalHandler}>
      <input type="text" />
      <button type="submit">Add Goal</button>
    </form>
  );
};

export default NewGoal;

// App.js

import NewGoal from './components/NewGoal/NewGoal';

const addNewGoalHandler = (newGoal) => {
  courseGoals.push(newGoal);
  console.log(courseGoals);
}

<NewGoal onAddGoal={addNewGoalHandler} />
<GoalList goals={courseGoals} />

```

## State

* React does not rerender the JSX code when an event happens, you have to tell react when to rerender.
  * This is done by state.
  * Whenever we change the array, it should update the UI of the component when we update it.
* We use `import React, { useState } from 'react';`
  * This is a functional component react hook that we use to manage state.
  * You can have multiple useState cases, and they are watched independently.
    * useState returns an array of exactly two elements: An initial state snapshot, a function that allows us to update the state.

```javascript

import React, { useState } from 'react';

const App = () => {
  const [courseGoals, setCourseGoals] = useState([
    {id: 'cg1', text: 'One Goal'},
    {id: 'cg2', text: 'Two Goals'},
    {id: 'cg3', text: 'Three Goals'},
  ]);

  const addNewGoalHandler = (newGoal) => {
    // setCourseGoals(courseGoals.concat(newGoal));
    setCourseGoals((prevCourseGoals) => {
      return prevCourseGoals.concat(newGoal);
    }));
  }
}

```
* Whenever you update that state snapshot, React will do two things:
  * Replace the initial state with our brand new state.
  * Call the component function again and execute the function again.
    * Under the hood, it will only update the dom elements that have changed.
* When you set a new state, it doesn't necessarily immediately rerender your app.
  * The state update might be deferred by a few miliseconds.
  * If your state update requires the previous state, you need to use the return option.
