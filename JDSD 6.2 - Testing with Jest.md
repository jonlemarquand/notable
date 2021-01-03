---
title: JDSD 6.2 - Testing with Jest
created: '2020-04-25T10:01:44.118Z'
modified: '2020-04-25T13:23:06.166Z'
---

# JDSD 6.2 - Testing with Jest

## Setting Up Jest

* `npm install --save-dev jest`
  * Newer versions of npm assume jest as a dev dependency.

```javascript

const googleDatabase = [
  'cats.com',
  'souprecipes.com',
  'flowers.com',
  'animals.com'
  'catpictures.com',
  'myfavouritecats.com'
];

const googleSearch = (searchInput, db) => {
  const matches = db.filter(website => {
    return website.includes(searchInput);
  })
  return matches.length > 3 ? matches.slice(0, 3) : matches;
}

console.log(googleSearch('cats', googleDatabase));
module.exports = googleSearch;
```
* Jest automatically looks for a folder called tests as well as spec.js and test.js files.

### Writing it() functions

```
it('this is a test', () => {

})
```

## Our First Tests

```
const googleSearch = require('./script');

dbMock = [
  'dog.com',
  'cheesepuff.com',
  'disney.com',
  'dogpictures.com'
];

it('tis searching google', () => {
  expect(googleSearch('testtest', dbMock)).toEqual([])
  expect(googleSearch('dog', dbMock)).toEqual(['dog.com', 'dogpictures.com'])
})

```
* With jest we can use `"test": "jest --watch *.js"`

## Writing Tests

* They never go into production, so it never hurts to have a lot of tests.
* Don't worry about the DRY principle with tests.
* Just because the test passes doesn't mean its correct.
  * Always worth making it fail first just to check the inputs.
* We can also use describe to group the functions together.
  * This makes scanning the list easier to do.

```javascript

describe('googleSearch', () => {
  it('work with undefined and null input', () => {
    expect(googleSearch(undefined, dbMock)).toEqual([]);
    expect(googleSearch(undefined, dbMock)).toEqual([]);
  })

  it('does not return more than 3 matches', () => {
    expect(googleSearch('.com', dbMock).length).toEqual(3);
  })
})

```

## Asynchronous Tests

* Fetch in node doesn't work because it is used on the window object.
  * You can install `npm install node-fetch`

```javascript

// script2.js

const fetch = require('node-fetch');

const getPeoplePromise = fetch => {
  return fetch('https://swapi.co/api/people')
    .then(response => response.json())
    .then(data => {
      return {
        count: data.count,
        results: data.results
      }
    })
}

const getPeople = async(fetch) => {
  const getRequest = await fetch('https://swapi.co/api/people');
  const data = await getRequest.json();
  console.log(data);
  return {
    count: data.count,
    results: data.results
  }
}

module.exports = {
  getPeople,
  getPeoplePromise
}

// script2.test.js

const fetch = require('node-fetch');
const swapi = require('./script2');

it('calls swapi to get people', (done) => {
  expect.assertions(1)
  swapi.getPeople(fetch).then(data => {
    expect(data.count).toEqual(87)
    done();
  })
})

```
* With this fetch test, if we didn't use `expect.assertions(1)` and `done()`, this code would pass because getPeople was called rather than waiting for the results.
* THe other way of getting around, we return the promise.
  * The test will automatically fail if the promise isn't returned.
* Jest Cheat Sheet: https://github.com/sapegin/jest-cheat-sheet


## Mocks and Spies

* With a mock, we can fake a function and pretend that it is running.
* To create a mock:

```javascript

it('getPeople returns count and results', () => {
  const mockFetch = jest.fn().mockReturnValue(Promise.resolve({
    json: () => Promise.resolve({
      count: 87,
      results: [0,1,2,3,4,5]
    })
  }))
  expect.assertions(2)
  return swapi.getPeoplePromise(mockFetch).then(data => {
    expect(mockFetch.mock.calls.length).toBe(1);
    expect(mockFetch).toBeCalledWith('https://swapi.co/api/people');
  })
})

```
* You can spy on the `mockFetch` const because it was created with a `jest.fn()`.
  * Called a spy because it lets you spy on the behaviour of the function.
