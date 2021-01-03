---
title: VueJS 4-5 - Under the Hood
created: '2020-10-30T14:32:52.882Z'
modified: '2020-10-30T14:51:10.953Z'
---

# VueJS 4-5 - Under the Hood

* Just as you can access data methods through the `this` keyword, you can also access methods.

## Working with refs

```html

<input type="text" ref="userText">

```

```javascript
// This will only work when the method is called.
setText() {
  this.message = this.$refs.userText.value
}

```

## Vue App Lifecycle

createApp -> beforeCreate() -> created()-> compile template -> beforeMount() -> mounted() -> mounted vue instance -> data changed -> beforeUpdate() -> updated()

* beforeCreated is called before the app is fully initialised
* created is after
* beforeMount is before the thing appears on screen
* updated is when the new content will be visible.
* An instance could be unmounted // all it's content is removed from the screen and its app is dead.
  * beforeUnmount() and unmounted() are also hooks that can be used.

### How to use

* These methods mentioned above can simply be referenced in methods.
