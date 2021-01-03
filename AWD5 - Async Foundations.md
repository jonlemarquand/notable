---
title: AWD5 - Async Foundations
created: '2020-03-25T14:06:17.260Z'
modified: '2020-03-25T14:35:23.743Z'
---

# AWD5 - Async Foundations

## Callback Functions

* Objectives:
  * Define callback functions
  * Define higher order functions
  * Use a callback function to make your code more general
  * Create callbacks using anonymous functions
* A callback function is a function that is passed into another function as a parameter then invoked by that other function.

```javascript

function callback() {
  console.log("Coming from callback");
}

function higherOrder(fn) {
  console.log("About to call callback");
  fn();
  console.log("Callback has been invoked");
}

higherOrder(callback);

```
* A higher order function is a function that accepts a function as a parameter.
* What are callbacks used for?
  * Advanced array methods
  * Browser events
  * AJAX requests
  * React development

* Callbacks with Anonymous Functions:

```javascript

function greet(name, formatter) {
  return "Hello, " + formatter(name);
}

greet("Tim", function(name) {
  return name.toUpperCase();
});

// will return Hello TIM

```

## The Stack and the Heap

* The stack is an ordered data structure
  * Keeps track of function invocations
  * Part of the JavaScript runtime (You don't access it directly)

