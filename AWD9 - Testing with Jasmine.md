---
title: AWD9 - Testing with Jasmine
created: '2020-03-25T14:40:40.004Z'
modified: '2020-03-25T18:33:26.537Z'
---

# AWD9 - Testing with Jasmine

## Writing Tests in the Browser

* Why? Because everyone makes mistakes but some are preventable.
* Unit tests test parts or units of an application.
* Jasmine
  * Works with all kinds of javascript environments.
  * Simple syntax to quickly get up and running with.
  * Similar to other testing libraries.

## Jasmine Syntax and Matchers

* `describe` - this function is used to organise test.
  * let me describe something to you.
* `it` - inside describe functions.
  * inside this function we write code with what we expect to happen.
* `expect` - lives inside the it function
  * where we make expectations about the function
  * if one of the expectations isn't met, the test fails.
* A conceptual exercise.
  * describe ("Earth")
    * it ("Is round")
      * expect(earth.isRound.toBe(true))

```javascript

var earth = {
  isRound: true,
  numberFromSun: 3
}

describe("Earth", function(){
  it("is round", function(){
    expect(earth.isRound).toBe(true)
  });
  it("is the third planet from the sun", function(){
    expect(earth.numberFromSun).toBe(3);
  });
});

```

* Spec is another word for test.
  * Each it function we use is considered a test
*  Generally have an external javascript file to test.

### Matchers
  
  * Functions that we attach to the result of the expect function.
  * `toBe` / `not.toBe` uses === to compare one value to another.
  * toBeClostTo - For things that are similar but not the same.
  * toBeDefined - helpful when making sure certain variables have a speicif value and not undefined.
  * toBeTruthy/toBeFalsey
  * toBeGreaterThan
  * toContain - Is there a value in an array?
  * toEqual - This is how we compare two different objects, and compares if their values are the same rather than different references in memory.
  * jasmine.any() - Helpful tool when doing type checking.
    * When we want to check if some value is an array or object.

## Writing Better tests with Hooks

* BeforeEach accepts a callback function that will run before the "it" callback runs.
  * This can be used to insert an array into tests.
* AfterEach
  * After the function, you can reset variables used between tests.
* beforeAll / afterAll
  * The arr variable only gets defined once and is shared throughout all the tests.
* Nesting describe
  * We can nest describe blocks if we are describing a lot of things.
* Pending tests
  * When we don't know what we'll be testing or we don't want to run a specific test.
  * Omitting a callback function to the it function, Adding a pending function or placing an x before the it function.
* One or more expect
  * If the testing of one unit needs more than one expect use them.

## Spies

* When unit testing, we strive to isolate specific functionality and how it behaves under a variety of circumstances.
  * However, they often rely on other parts of the data.
  * A mock is a fake object that poses as a function without having to create the actual object.
    * Mocks can be used to retrieve various things about the object/function.
* In Jasmine, mocks are referred to as spies.
  * A spy can stub (mimic) any function and track calls to it and all arguments.
  * Spies only exists in the describe or it block in which it is defined.
  * Spies are removed after each spec.

