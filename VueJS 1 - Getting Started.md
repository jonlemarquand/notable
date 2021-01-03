---
title: VueJS 1 - Getting Started
created: '2020-10-29T15:34:06.996Z'
modified: '2020-10-29T16:36:30.365Z'
---

# VueJS 1 - Getting Started

* Vue can be used to control parts of HTML pages or entire pages.
* Vue can be used to control the entire frontend of a web application.

## Vanilla Javascript vs Vue

### Vanilla Javascript:

```javascript

const buttonEl = document.querySelector('button');
const inputEl = document.querySelector('input');
const listEl = document.querySelector('ul');

function addGoal() {
  const enteredValue = inputEl.value;
  const listItemEl = document.createElement('li');
  listItemEl.textContent = enteredValue;
  listEl.appendChild(listItemEl);
  inputEl.value = '';
}

buttonEl.addEventListener('click', addGoal);

```

### Vue App

```html
<div id="app">
  <input type="text" id="goal" v-model="enteredValue">
  <button v-on:click="addGoal">Add Goal</button>
  <ul>
    <li v-for="goal in goals">{{ goal }}</li>
  </ul>
</div>
```
```javascript
// vue
// before app.js in html file: <script src="https://unpkg.com/vue@next"></script>

Vue.createApp({
  data() {
    return {
      goals: [],
      enteredValue: ''
    };
  },
  methods: {
    addGoal() {
      this.goals.push(this.enteredValue);
      this.enteredValue = '';
    }
  }
}).mount('#app');

```
* The data has a function as a value, and must return an object.
  * The data is a bit like state.
  * Using v-model will connect the data and input value.
* Vue establishes the connection on `this` value.
* This list item will render as needed.


## Setting up the Environment

* Preferences > Shortcuts > Format Document or use prettier
