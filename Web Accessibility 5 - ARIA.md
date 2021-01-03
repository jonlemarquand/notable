---
title: Web Accessibility 5 - ARIA
created: '2020-09-12T10:57:35.950Z'
modified: '2020-09-12T11:14:22.232Z'
---

# Web Accessibility 5 - ARIA

## Why ARIA

* Native elements give you a lot of built in support for free, but sometimes you need custom elements.
* wai-aria - Web Accessibility Initiative Accessible Rich Internet Applications
* Aria allow you to specify attributes on elements which modify how items are read by screen readers and other assistive technologies.
  * add `role="checkbox" aria-checked="true"` to make it a custom aria tagged.
  * Aria checked labels have to be declared explicitly.
* All ARIA does is modify the accessibility tree, not any of the functionality.

### What can ARIA do for you?

* ARIA can modify certain semantics within existing elements.
* ARIA can send alerts to screen readers.

## Roleplaying

* A role refers to shorthand for a particular design pattern.
  * When we were saying 'role=checkbox', we're telling assistive technology it follows the checkbox role.
* ARIA roles: https://www.w3.org/TR/wai-aria-1.0/#roles

```html

<button aria-label="menu" class="hamburger">

<button aria-label="close">X</button>

```
* This overrides any other label element.
* `aria-labelledby` allows us to specify another element's id to give the label. Any element.
  * aria-labelledby always takes precedence.
