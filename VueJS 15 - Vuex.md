---
title: VueJS 15 - Vuex
created: '2020-11-16T13:10:13.871Z'
modified: '2020-11-16T13:17:35.038Z'
---

# VueJS 15 - Vuex

## What & Why

* Vuex is a library for managing global state.
* Local state: The data you manage inside one component, but only effects one components/child components.
  * E.g. user input, button that show/hides container.
* Global state: Affects multiple components/entire app.
  * E.g. user authenitcation, shopping cart items etc.
  * The global state is where Vuex can help.
* Provide/inject can have tricky problems regarding reactivity.

### Why Vuex?

* Managing app-wide/global state can be difficult.
  * You end up with "fat components"
  * Unpredictable: It's not always obvious where data gets changed in which way
  * Error-prone: Accidental or missed state updates are possible.
* With Vuex:
  * Outsourced state management.
  * More predictable
    * Vuex has clear rules on how to manage state and how it can be edited.

## Creating & Using a Store

* `npm install --save vuex` or add @next to the end for the latest version.

