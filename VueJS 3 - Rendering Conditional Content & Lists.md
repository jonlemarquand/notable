---
title: VueJS 3 - Rendering Conditional Content & Lists
created: '2020-10-30T11:33:36.591Z'
modified: '2020-10-30T12:39:48.361Z'
---

# VueJS 3 - Rendering Conditional Content & Lists

## Rendering Content Conditionally

```html

<p v-if="goals.length === 0">Test</p>

```
* If the v-if condition is met, the content will be rendered.

### v-if, v-else and v-else-if

* You can use whatever conditions you want, as you would with javascript on the v-if values.
* Instead of repeating the if, you can also use v-else.
  * v-else has to be used on an element used directly after an element with v-if in it.

### v-show rather than v-if

* v-show only works standalone, so v-else doesn't work with it.
* with v-show, the element will be hidden rather than removed from the page.
  * Adding and removing elements can cost performance.
  * But you can have elements in the DOM that you aren't using.
* If an element with a visibility changing all the time, you can use v-show.

## Rendering Lists of Data

* To repeat a list item, you can use `v-for`:

```html

<li v-for="goal in goals">{{ goal }}</li>

```
* You can pull out more than just the array item, you could do the index of the array item:

```html

<li v-for="(goal, index) in goals">{{ goal }}</li>

```
* You can also loop through objects:

```html

<li v-for="(value, key, index) in {name: 'Max', age: 31}">{{key}}: {{ value }}</li>
<!--Will out put 'name: Max' and 'age: 31' as two items.-->
```
* Loop through numbers:

```html

<li v-for="num in 10">{{ num }}</li>

```

### Removing List Items

```html

<li 
  v-for="goal in goals" 
  @click="removeGoal(index)"
>
  {{ goal }} - {{ index }}
</li>

```

### Lists & Keys

```html
<li 
  v-for="goal in goals" 
  @click="removeGoal(index)"
>
  <p>{{ goal }} - {{ index }}</p>
  <input type="text" @click.stop>
</li>
```
* This does produce a bug in that the input text will remain on the 'index 1', even if it is deleted.
* Or alternatively:
```html
<li 
  v-for="goal in goals" 
  @click="removeGoal(index)"
  :key="goal"
>
  <p>{{ goal }} - {{ index }}</p>
  <input type="text" @click.stop>
</li>

```
* In most cases there will be a unique id you can use for key, but it's good practice as index can change.
* Vue will then be able to tell the DOM elements apart.


