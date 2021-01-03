---
title: Javascript 8 - Asynchronous Javascript
created: '2020-03-18T07:46:59.091Z'
modified: '2020-03-19T06:58:46.347Z'
---

# Javascript 8 - Asynchronous Javascript

## Understanding Asynchronous Javascript

* Synchronous - All the Javascript is executed line by line, as it appears in the code.
* Asynchronous - We don't wait for a code to finish executing but move on so other code can keep running.
  * Allow asynchronous functions to run in the "background"
  * We pass in callbacks that run once the function has finished its work.
  * Move on immediately: Non-blocking!

## The Old Way: Asynchronous Javascript with Callbacks

```javascript

function getRecipe(){
  setTimeout(() => {
    const recipeID = [523, 883, 432, 974];
    console.log(recipeID);
    setTimeout(id => {
      const recipe = {title: 'Fresh tomato pasta', publisher: 'Jonas'};
      console.log(`${id}: ${recipe.title}`);
    }, 1000, recipeID[2]);
  }, 1500)
}
getRecipe();
// This will return the ids after 1.5 seconds
// Then the recipe after 1 second
```
* However, this process can lead to the callback hell.
  * A callback/set timeout function within a function within a function.

## From Callback Hell to Promises

* Promise: Object that keeps track about whether a certain event has happened already or not.
  * Determines what happens after the event has happened.
  * Implements the concept of a future value that we're expecting.
* Promise States
  * Pending - Before the event happens
  * Settled/Resolved - After the event happens.
    * Fulfilled - If the promise worked.
    * Rejected - If the promise did not work.

```javascript

const getIDs = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([523, 883, 432, 974])
  }, 1500);
});

getIDs.then(IDs => {
  console.log(IDs);
}); // the callback to execute when the promise is returned

getIds.catch(error => {
  console.log(error);
})

```
* We created a promise with an exectuor function
  * We then put some asynchronous data in.
  * We then use he resolve function returns the result.
  * We can then use the `.then` method which will do something with the successful get.
  * There is also the `catch` method if there was an error in the promise.
* Using multiple promises:

```javascript

const getIDs = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([523, 883, 432, 974])
  }, 1500);
});

const getRecipe = recID => {
  return new Promise((resolve, reject) => {
    setTimeout(ID => {
      const recipe = {title: 'Fresh tomato pasta', publisher: 'Jonas'};
      resolve(`${id}: ${recipe.title}`);
    }, 1500, recID);
  });
};

getIDs
  .then(IDs => {
    console.log(IDs);
    return getRecipe(IDs[2]);
  }) // the callback to execute when the promise is returned
  .then(recipe => {
    console.log(recipe);
  })
  getIds.catch(error => {
    console.log(error);
  });

```

## From Promises to Async/Await

* ES8 changed how we consume promises.
* Await expression can only be run in the async function.
* The async function always runs in the background.
* This code below will wait at the `await` until the getIDs has run.
  * IDs will then become the result of getIDs if the promise is received.

```javascript

async function getRecipesAW(){
  const IDs = await getIDs;
  console.log(IDs);
  const recipe = await getRecipe(IDs[2]);
  console.log(recipe);
}
getRecipesAW();

```

## AJAX and APIs

* AJAX: Asynchronous Javascript and XML
  * Allows us to asynchronously communicate with servers.
  * Get data without having to reload the page.
* APIs: Application Programming Interface
  * A piece of software used by another piece of software to allow programs to talk to each other.
  * API is like the front end of the backend.
  * Your own API data comign from the server or 3-rd party APIs such as google maps.

### Making AJAX Calls with Fetch and Promises

```javascript
// The fetch API gets our data and returns a promise.
fetch('https://crossorigin.me/https://www.metaweather.com/api/location/2487956/')
.then(result => {
  console.log(result);
  return result.json();
})
.then(data => {
  const today = data.consolidated_weather[0];
  console.log(`Temperatures in ${data.title} stay between ${today.min_temp} and ${today.max_temp}.`);
})
. catch(error => {
  console.log(error);
});
```

### Making AJAX Calls with Async/Await

```javascript

async function getWeatherAW(woeid){
  try {
    const result = await fetch(`https://crossorigin.me/https://www.metaweather.com/api/location/${2487956}/`);
    const data = await result.json();
    console.log(`Temperatures in ${data.title} stay between ${today.min_temp} and ${today.max_temp}.`);
  } catch(error) {
    alert (error);
  }
}
getWeatherAW(44418);

```

* The try/catch statmement is async's `if/else` version of the resolve/reject of promises.
