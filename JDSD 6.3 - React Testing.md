---
title: JDSD 6.3 - React Testing
created: '2020-04-26T09:55:32.870Z'
modified: '2020-04-27T07:06:44.184Z'
---

# JDSD 6.3 - React Testing

## Introduction to Enzyme

* How do you test React components?
  * Enzyme - A library by AirBnb which is quite standard in React community.
* In src > `setupTests.js`:

```javascript

import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16'

configure({ adapter: new Adapter() });

```
* Enzyme gives us three things to use: `import { shallow, mount, render } from 'enzyme';`

```javascript
// Card.test.js

import React from 'react';
import { shallow, mount, render } from 'enzyme';
import Card from './Card';

it('expect to render Card component', () => {
  expect(shallow(<Card />).length).toEqual(1)
})
```
* `run npm tests`
* Shallow rendering just renders the card component.
  * It would only render this component, rather than other components that were in the card.
  * It is one unit test, that just tests one unit.
* Mount does a full DOM rendering.
  * Where you have components that interact with the DOM API.
  * If the card component has some sort of lifecycle method.
  * It requires a full DOM API to be available to work.
    * It has to run in an environment that looks like a browser.
  * Mounts the component on a DOM, like React does.
* Render is used to render react components to a static html.
  * Render is between shallow and mount.
  * Renders children components as well.

## Snapshot Testing

* Pure function components could lead to repeated tests.
  * What if we could take a snapshot of what the card component renders.
    * Then if when we develop and something changes, it would fail the test because it wouldn't match the snapshot.

```javascript
// Card.test.js

import React from 'react';
import { shallow, mount, render } from 'enzyme';
import Card from './Card';

it('expect to render Card component', () => {
  expect(shallow(<Card />)).toMatchSnapshot();
})
```
* This method will create a snapshot folder, with snapshots in, created by jest.
  * This means that if a developer comes in and messes up the card component, it would then fail.
  * When you run the test, you have the option to update the snapshots. If you have changed the card intentionally.
* `npm test -- --coverage`
  * This shows the code coverage of the tests.
* For a more complicated component:

```javascript

it('expect to render CardList component', () => {
  const mockRobots = [
    {
      id: 1,
      name: 'John Snow',
      username: 'JohnJohn',
      email: 'john@gmail.com'
    }
  ]
  expect(shallow(<CardList robots={mockRobots}/>)).toMatchSnapshot();
})

```

## Testing Stateful Components

```javascript

// CounterButton.test.js

it('expect to render CounterButton component', () => {
  const mockColor = 'red'
  expect(shallow(<CounterButton color={mockColor}/>)).toMatchSnapshot();
})

it('correctly increments the counter', () => {
  const mockColor = 'red'
  const wrapper = shallow(<CounterButton color={mockColor} />);
  wrapper.find('[id="counter"]').simulate('click');
  expect(wrapper.state()).toEqual({ count: 1 });
})

```

## Testing Connected Components

```javascript

// App.test.js

let wrapper;

beforeEach(() => {
  const mockProps = {
    onRequestRobots: jest.fn();
    robots: []
    searchField: '',
    isPending: false
  }
  wrapper = shallow(<MainPage { ...mockProps}/>)
})

it('renders MainPage without crashing', () => {
  expect(wrapper).toMatchSnapshot();
})

it('filters robots correctly 2', () => {
  const mockProps2 = {
    onRequestRobots: jest.fn();
    robots: [{
      id: 3,
      name: 'John',
      email: 'john@gmail.com'
    }],
    searchField: 'john',
    isPending: false
  }
  const wrapper2 = shallow(<MainPage { ...mockProps2}>)
  expect(wrapper2.instance().filterRobots()).toEqual([{
    id: 3,
    name: 'John',
    email: 'john@gmail.com'
  }]);
})

```
* `redux-mock-store` npm package.
* Other option: Simplicity.
  * Often in order to test things, we have to simplify our code.


## Testing Reducers

* Reducers are pure functions, and pure functions are a testers dream.

```javascript

import {
  CHANGE_SEARCHFIELD,
  REQUEST_ROBOTS_PENDING,
  REQUEST_ROBOTS_SUCCESS,
  REQUEST_ROBOTS_FAILED
} from './constants';

import * as reducers from './reducers';

describe('searchRobots', () => {
  const initialStateSearch = {
    searchField: ''
  }

  it('should return the initial state', () => {
    expect(reducers.searchRobots(undefined, {})).toEqual({searchField: ''})
  })

  it('should handle CHANGE_SEARCHFIELD', () => {
    expect(reducers.searchRobots(initialStateSearch, {
      type: CHANGE_SEARCHFIELD,
      payload: 'abc'
    })).toEqual({searchField: 'abc'})
  })
})

```

## Testing Actions

```javascript
import * as actions from './actions';
import {
  CHANGE_SEARCHFIELD,
  REQUEST_ROBOTS_PENDING,
  REQUEST_ROBOTS_SUCCESS,
  REQUEST_ROBOTS_FAILED
} from './constants';
import configureMockStore from 'redux-mock-store';
import thunkMiddleware from 'redux-thunk';

const mockStore = configureMockStore([thunkMiddleware])

it('should create an action to search robots', () => {
  const text = 'wooo';
  const expectedAction = {
    type: CHANGE_SEARCHFIELD,
    payload: text
  }
  expect(actions.setSearchField(text)).toEqual(expectedAction);
})

it('handles requesting robots API', () => {
  const store = mockStore();
  store.dispatch(actions.requestRobots())
  const action = store.getActions();

  const expectedAction = {
    type: REQUEST_ROBOTS_PENDING,
  }
  expect(actions[0]).toEqual(expectedAction)
})

```
* For the second one, we need to use `npm install --save-dev redux-mock-store`
  * Because the actual action uses middleware
* `nock` - You can mock API calls for Async tests.
* `supertest` - you can also help test for APIs.

