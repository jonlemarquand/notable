---
title: JDSD 6.1 - Testing Introduction
created: '2020-04-24T07:16:51.064Z'
modified: '2020-04-25T10:01:42.163Z'
---

# JDSD 6.1 - Testing Introduction

* TDD - Test Driven Development
  * Writing tests before the code.
* 3 Main Categories:
  * Unit Tests - Tests individual functions + classes.
    * `a + b = c`
  * Integration Tests - Testing how different pieces of code work together.
  * Automation Tests - Testing real life scenarios on the browser.
1. A Testing Library
  * Scaffolding/structure around the test.
  * Jasmine/Jest/Mocha
2. Assertion Library
  * A tool that allows you to test that the variable contains the expected value.
  * Jasmine/Jest/Chai
3. A test runner
  * Something that allows us to run our tests
  * Jasmine/Jest both include a test runner
  * Mocha has one to
  * Karma allows you to run tests in the browser.
    * Can use with Puppeteer (From Google)
    * Also use jsdom (A Javascript like version of the DOM)
4. Mock Spies/Stubs
  * Spies provide information about functions
  * Stubbing replaces selected functions with a function. It fakes functions.
    * Like faking the server code, so we don't have to run a server every time.
  * Jasmine/Jest
  * Sinon.js
5. Code coverage
  * Istanbul/Jest
* Jasmine used to be more popular, but has been replaced by Jest/Mocha(w/Chai/Sinon)
  * Jest is from Facebook.
* create-react-app comes with the testing libraries out of the box.
* Snapshot testing: Unique to react.
* Enzyme: Developed by AirBNB to test react libraries.

## Unit Tests

* Small pure functions should be unit tested.
  * A pure function has no side effects.
  * It is deterministic: It should always give the same output to the input.
* Give it an input, get an output.
* A lot of react components can be tested with these.
  * Because they get a prop, they get a view.
* Unit tests don't test the contract between apps (the database and express for example)

## Integration Tests

* Integration tests are about cross-communication between different pieces of code.
  * Use spies/stubs to ensure bits are working.
  * Mocking a database call with stubs. (Return a fake user just for the test.)
* A browser could be helpful.
* Connecting components to see how they work together.
  * They are expensive and slower.
  * More dev time involved in building them.
  * They also have a lot of moving parts.

## Automation Tests

* End-to-end testing
* UI Tests that are always running in a browser or a browser like environment.
* Need to make sure these scenarios work from the point of view of an end-user.
  * These tests are the hardest to set up.
* Unit tests/integrations tests every time you save your code.
* UI tests - every week / maybe on merge into master branch.
