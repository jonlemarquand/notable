---
title: VueJS 14 - Animations & Transitions
created: '2020-11-10T15:00:44.901Z'
modified: '2020-11-11T21:54:54.328Z'
---

# VueJS 14 - Animations & Transitions

* You can wrap an element (one direct child) in `<transition>` which will then manipulate the element
* It adds an enter-from, enter-to and enter-active classes.
  * Also has leave-from, leave-to and leave-active on exit from mount.
  * Needs to have at least one transition for Vue to work out timings.

```css

.v-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}
.v-enter-active {
  transition: all 0.3s ease-out;
}
.v-enter-to {
  opacity: 1;
  transform: translateY(0);  
}

```

```css

.v-enter-active {
  animation: slide-scale 0.3s ease-out;
}

/* css animation here */

```
## Custom named components

* `<transition name="para">` then `para-enter-active` etc in the css for multiple animations
* Could also define by `enter-to-class="some-class`

## Transitioning between multiple elements

* The one exception for more than one direct child element: If you guarantee only one of the child elements will be added at a time.
  * A Show button and a hide button but used with v-if and v-else rather than two v-ifs.
  * `mode="out-in"`

## Using Transition Events

* `@before-enter=""` - This method will run when the enter-from is being run.
  * Only fires when the element enters.
  * `@before-leave=""` is the equivalent.
* In these methods you get the argument `el` in `beforeEnter(el)` that runs the element which is added.
  * So, for example, you can control the element's css through javascript.
* Also have a `@enter=""` which is equivalent to the active CSS class.
  * And `@after-enter` as well as leave equivalents.
* Triggers events appropriately in line with transition css time set.

## Building Javascript transitions (Instead of CSS)

* You could use:

```javascript

methods: {
  beforeEnter(el) {
    el.style.opacity = 0;
  },
  enter(el, done) {
    let round = 1
    const interval = setInterval(function() {
      el.style.opacity = round * 0.1
      round ++;
      if (round > 10) {
        clearInterval(interval);
        done();
      }
    }, 20);
  }
}

```
* The done function is useful if you aren't using CSS for Vue to parse a time.
* To prevent both enter and leave happening you can use @enter-cancelled="enterCancelled" and @leave-cancelled=""
  * `enterCancelled(el) { clearInterval = this.enterInterval }

### Disabling css transitions

* `<transition :css="false">`
* Technically not required, but Vue now won't even search for it.

## Animated Lists

```html

<transition-group>
  <li v-for="user in users" :key="user" @click="removeUser(user)">
</transition-group>

```
* Transition group will render an element to the DOM.
* You can give it the `tag="ul"` but can use any html element.

### Animate List Item Movement

* When you are using transition-group, you can use: `.prefix-move`

```css

.prefix-list-move {
  transition: transform 0.8s ease;
}

```
* On the leave active, you need to add a position of absolute.

## Animating Route Changes

```html

// Vue 3

<router-view v-slot="slotProps">
  <transition name="route" mode="out-in">
    <component :is="slotProps.Component"></component>
  </transition>
</router-view>

```
* If you don't want an animation straight away, go to main.js and use `router.isReady().then(function() { app.mount('#app'); });`
