---
title: VueJS 9 - Diving Deeper into Components
created: '2020-11-01T12:57:38.890Z'
modified: '2020-11-02T07:09:06.965Z'
---

# VueJS 9 - Diving Deeper into Components

## Global vs Local Components

* You don't have to register all components globally (on main.js)
* Global:
  * Can use them as HTML elements anywhere in the app.
  * Useful for repeating elements.
  * They all need to be loaded when your application loads initially.
  * Not obvious where these components are getting used.
* Local:
  * Register them in the component where we plan on using them.
  * In the script section, you can use `import TheHeader from './someAddress/TheHeader.vue'`

```javascript

<template>
  <TheHeader />
</template>

<script>

import TheHeader from './components/TheHeader.vue';

export default {
  components: {
    'the-custom-html-element': TheHeader,
    TheCustomHtmlElement: TheHeader
    TheHeader //if you are using the same value for key-value
  },
}

</script>

```

## Scoped Styles

```html

<style scoped>
</style>

```
* This will only effect the template that is in the file.

## Slots

* A standalone component, with styling that doesn't have content but can be reused.
  * For example, a card section which receives content.
  * A wrapper with a certain style attached around dynamic content.

```html

<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
  export default {
  }
</script>

<style scoped>
</style>
```

### Named Slots

* What if you want a header slot and content slot?
* If you leave one unamed slot, that will be the default slot.

```html

<template>
  <header>
    <slot name="header"></slot>
  </header>
</template>

<template>
  <base-card>
    <template v-slot:header>
      <h3>Name</h3>
    </template>
  </base-card>
</template>

```

* If you add content between the `<slot>` tags, this will become the fallback default content if nothing else is there.
* In the component where you define slots, you get a special view: `$slots`:

```html
<header v-if="$slots.header"> <!-- Will return 'undefined', therefore false -->
    <slot name="header>
    </slot>
</header>

```

### Scoped Slots

* Letting you pass data inside components where you define data to components where you define markup.

```html
<!--CourseGoals.vue-->
<ul>
  <li v-for="goal in goals" :key="goal">
    <slot :item="goal"></slot>
  </li>
</ul>

<!--App.vue-->
<course-goals>
  <template #default="slotProps">
    <h2>{{ slotProps.item }}</h2>
  </template>
</course-goals>
```
* If there's only one slot, you don't need to use the template markup, can put it straight in `course-goals`

## Dynamic Components

* For dumb components, where it's literally just a template, you don't have to export them with script tags.
  * You can instead just import them in the script, and then declare them as components.
* Vue offers a special HTML element `<component></component>`
  * It needs an `is=""` property
  * This will swap itself for the component mentioned in is.

```html
<button>Active Content</button>
<button>Main Content</button>
<component :is="selectedComponent"></component>
```
* However, when you switch components, you will lose data when you switch away because it's removed from the DOM.
* To solve this, you can use `<keep-alive>` wrapped around the dynamic component.

## Teleporting Elements

* Teleport is a built in Vue component that can be wrapped around the to-be teleported object with `<teleport></teleport>`
* It wants one prop, you provide a CSS selector where the object should be added.
  * Such as `body` or `#app`

## Working with Fragments

* In vue 2, similar to React, you had to have a wrapping element but in Vue 3 you can now have multiple top-level elements.

## Vue Style Guide

* There is an official Vue Style guide: Recommendations for code and naming.








