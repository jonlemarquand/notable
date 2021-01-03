---
title: VueJS 2 - Basics & Core Concepts (DOM Interaction with Vue)
created: '2020-10-29T16:41:28.611Z'
modified: '2020-10-30T11:31:44.365Z'
---

# VueJS 2 - Basics & Core Concepts (DOM Interaction with Vue)

## Creating and Connecting Vue App Instances

1. Create a Vue App

* If we control a HTML element with Vue, we'll also control all child elements of that element.
* IDs are a great thing to use, as there should only be one per page.
* The name has to be data, Vue uses specific property names.
  * Data always returns an object, but within that you can set up any key/value pair you want.

```javascript

const app = Vue.createApp({
  data() {
    return {
      courseGoal: 'Finish the course and learn Vue'
    };
  }
});

app.mount('#user-goal');

```

```html

<section id="user-goal">
    <h2>My Course Goal</h2>
    <p>{{ courseGoal }}</p>
</section>

```

* To output data, you use `{{ }}`

## Binding Attributes with 'v-bind' Directive

* The `{{ }}` syntax is only available between HTML tags.
* You don't always want to interpolate data. You could use:

```javascript

const app = Vue.createApp({
  data() {
    return {
      courseGoal: 'Finish the course and learn Vue',
      vueLink: 'https://vuejs.org/'
    };
  }
});

app.mount('#user-goal');

```

```html

<section id="user-goal">
    <h2>My Course Goal</h2>
    <p>{{ courseGoal }}</p>
    <a v-bind:href="vueLink">About Vue</a>
</section>

```
* All built-in directives that ship with vue start with `v-`
* V-bind tells Vue to set the value of the href attribute to something

## Understanding Methods in Vue Apps

* Methods allow you to define functions that will execute when you need them.

```javascript

const app = Vue.createApp({
  data() {
    return {
      courseGoal: 'Finish the course and learn Vue',
      vueLink: 'https://vuejs.org/'
    };
  },
  methods: {
    outputGoal() {
      const randomNumber = Math.random();
      if (randomNumber < 0.5) {
        return 'Learn Vue!';
      } else {
        return 'Master Vue!';
      }
    }
  }
});

app.mount('#user-goal');

```
```html

<section id="user-goal">
    <h2>My Course Goal</h2>
    <p>{{ outputGoal() }}</p>
    <a v-bind:href="vueLink">About Vue</a>
</section>

```
* In this HTML code, you can call methods, as long as they are defined in methods or do simple functions.

## Working with Data inside of a Vue App

* To use data inside a method, we can use `this.`
* Vue takes all the data and merges it in to a global instance. So when you call `this.` you can access anything stored in the global object.

```javascript

const app = Vue.createApp({
  data() {
    return {
      courseGoalA: 'Finish the course and learn Vue',
      courseGoalB: 'Master the course and be a Vue legend'
      vueLink: 'https://vuejs.org/'
    };
  },
  methods: {
    outputGoal() {
      const randomNumber = Math.random();
      if (randomNumber < 0.5) {
        return this.courseGoalA;
      } else {
        return this.courseGoalB;
      }
    }
  }
});

app.mount('#user-goal');

```

## Outputting Raw HTML Content with v-html

* If the Vue data has html code in it, you might want to output it as HTML with `<p v-html="courseGoalB"></p>` which is interpreted as HTML.
* You probably won't use it too often, and shouldn't because there are risks if you use it.

## Understanding Web Events

```html

<button v-on:click="counter++">Add</button>
<button>Remove</button>

```
```javascript

const app = Vue.createApp({
  data() {
    return {
      counter: 0,
    };
  },
});

```
* All the default events you can listen to on HTML elements you can use with `v-on:`
* In a really simple way, with Vue you declare where you want to update the counter, and where it should be projected to.
  * In vanilla javascript you'd have to write event listeners, a function to update the content.

### Events & Methods

* Really, you don't want to put logic in your html code. It should really be in your javascript code.
* To update:

```html

<button v-on:click="add">Add</button>
<button>Remove</button>

```
```javascript

const app = Vue.createApp({
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    add() {
      this.counter++
    }
  }
});

```
### Working with Event Arguments

```html

<button v-on:click="add(5)">Add 5</button>
<button>Remove</button>

```
```javascript

const app = Vue.createApp({
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    add(num) {
      this.counter + num;
    }
  }
});

```
### Using the Native Event Object

```html

<input type="text" v-on:input="setName($event, 'Le Marquand')">
<p>Your Name: {{ name }}</p>

```
```javascript
const app = Vue.createApp({
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    setName(event, lastName) {
      this.name = event.target.value + ' ' + lastName;
    },
    }
  }
});

```
* The order of methods doesn't matter.
* The browser passes an event object on input.
* Only the parts that change in the code are rerendered.
* You can use the built-in event, if you have other inputs, thanks to Vue's `$event`.

### Exploring Event Modifiers

```html

<form v-on:submit="submitForm">
  <input type="text">
  <button>Sign Up</button>
</form>

```
```javascript

methods: {
  submitForm(event) {
    event.preventDefault();
    alert('Submitted!');
  }
}

```
* This would be option 1, a simple 'preventDefault' to stop the form submitting.
* You could also use an event modifier:

```html

<form v-on:submit.prevent="submitForm">
  <input type="text">
  <button>Sign Up</button>
</form>

```
```javascript

methods: {
  submitForm(event) {
    alert('Submitted!');
  }
}

```
* There are a few modifiers that can be used with Vue.
* They can also change when the event occurs:

```html

<button v-on:click.right="add(5)">Add 5</button>
<button>Remove</button>

```
* This will only trigger when the right click is used.
* There are also key modifiers, that monitor when key strokes are used.

```html

<input type="text" v-on:input="setName($event, 'Le Marquand')" v-on:keyup.enter="confirmInput">
<p>Your Name: {{ name }}</p>

```

### Locking content with v-once

* If you want to preserve the intial state, you can use the `v-once` directive.
  * This will tell Vue the value should only be evalutated once. It won't be updated after the initial value.


## Reactivity

### Data Binding + Event Binding = Two-Way Binding

```html

 <section id="events">
  <h2>Events in Action</h2>
  <button v-on:click="add(10)">Add 10</button>
  <button v-on:click="reduce(5)">Subtract 5</button>
  <p>Result: {{ counter }}</p>
  <input type="text" v-bind="name" v-on:input="setName($event, 'Schwarzmüller')">
  <button v-on:click="resetInput">Reset Input</button>
  <p>Your Name: {{ name }}</p>
</section>

```
```javascript

const app = Vue.createApp({
  data() {
    return {
      counter: 0,
      name: ''
    };
  },
  methods: {
    setName(event, lastName) {
      this.name = event.target.value;
    },
    add(num) {
      this.counter = this.counter + num;
    },
    reduce(num) {
      this.counter = this.counter - num;
      // this.counter--;
    },
    resetInput() {
      this.name = '';
    }
  }
});

app.mount('#events');

```
* This is a pattern you might need, you might have inputs where you want to have the value a user entered but set the value the user entered.
  * This is a common pattern, so much so that Vue has a shortcut:

```html

<input type="text" v-model="name">

```
* The v-model wants the data object that it is managing for you.
  * A shortcut for v-bind:value and v-input.
  * Two-way binding because we are listening for the event and writing the value back to the input element.

### Methods used for Data Binding: How it Works

```javascript

methods: {
  outputFullname() {
    if (this.name === '') {
      return '';
    }
    return this.name + ' ' + 'Le Marquand';
  },
}

```
* With this we can leverage the fact we're using a method to return different values based on the circumstances.
* A method called like `{{ outputFullName() }}`, it will be reexecuted everytime because the object  might be in it.
  * From a performance perspective, this is a problem. It's not a bug, it's how Vue works.

### Computed Properties

* Computed Properties are like methods, but Vue is aware of their dependencies.
* You should name computed properties as you would your data properties.

```javascript

data() {
},
computed: {
  fullname() {
    if (this.name === '') {
      return '';
    }
    return this.name + ' ' + 'Le Marquand';
  },
  }
},

```
```html
<p> Your Name: {{ fullname }}</p>
```

* We use them like data rather than methods.
* Generally better for outputting properties than methods.

### Working with Watchers

* Watchers are a function you can tell Vue to execute when one of it's dependencies changes.
  * Similar to `useEffect` hook with React?
```javascript

data() {
  name: '',
},
watch: {
  // define a bunch of methods
  name(newValue, oldValue) {
    this.fullName = newValue + ' ' + 'Le Marquand';
  }
},

```
* Repeat a data or computed property method and the watcher will execute when something changes.
* The watched value is passed as a property in Vue.
* A watcher only watches one property, so you would have to create multiple watchers for multiple data.
  * A computed property would only have one function.
* The main scenario for watchers would be something like, when counter gets to 20, do something.
  * To update a data property, but not always do it.
  * Http requests if certain data changes.

### Methods vs Computed vs Watch

* Methods:
  * Use with event binding OR data binding.
  * Data binding: Method is executed for every "re-render" cycle of the component.
  * Use for events or data that really needs to be re-evaluated all the time.
* Computed:
  * Use with data binding.
  * Computed properties are only re-evaluated if one of their "used values" changed.
  * Use for data that depends on other data.
* Watch:
  * Not used directly in template.
  * Allows you to run any code in readtion to some changed data.
  * Use for non-data update you want to make.

### v-bind and v-on shortcuts

* You can use `@click` rather than `v-on:click`.
* If you do use it, use it for all events.
* For `v-bind:value=" "` you can do `:value="..."`
* No v-model shorthand.

## Dynamic Styling

* In Vue, we can change the style of things on the page in reaction to something.
* If you bind style or class dynamically, you can use a special Vue syntax and feed in an object.

```html

<div class="demo" :style="{borderColor: boxASelected ? 'red' : '#ccc' }" @click="boxSelected('A')"></div>

```

### Adding CSS Classes Dynamically

```html

<div
  class="demo"
  :class="{active: boxASelected}"
  @click="boxSelected('A')"
>
</div>

```
* The active class will be added if boxASelected is true.
* You can use both dynamic and hard coded classes.

### Clasess & Computed Properties

```javascript

computed: {
  boxAClasses() {
    return {active: this.boxASelected}
  }
}

```
```html

:class="boxAClasses"

```
* More useful for complex dode to do with additionals.

### Dynamic Classes: Array Syntax

```html
<div
  :class="['demo', {active: boxBSelected}]"
>
</div>

```
