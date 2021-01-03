---
title: VueJS 11 - Forms
created: '2020-11-02T07:31:15.809Z'
modified: '2020-11-07T09:58:26.303Z'
---

# VueJS 11 - Forms

## v-model and inputs

```html

<form @submit.prevent="submitForm">
</form>

```
* If v-model is used on a type=number input, it will convert the value to a number type.
* You can force it by using `v-model.number`
* v-model.lazy allows you to update at a lower frequency.
* v-model.trim allows you to get rid of whitespace at the start and end
* v-model also works on selects.
* With checkboxes and radio buttons, add v-model on every button.
  * Checkboxes need values,

## Basic Form Validation

* The blur event, when an input loses focus.

```html

<button @blur="validateInput">

```
* This would then run `validateInput` which will check to see if its a valid username.

## Building a Custom Control Component

* Using v-model on a custom component is like `:model-value=""` and `@update:modelValue=""`
  * Bind a prop and emits that event.
