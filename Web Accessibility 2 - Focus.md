---
title: Web Accessibility 2 - Focus
created: '2020-09-05T14:53:16.000Z'
modified: '2020-09-12T11:00:32.913Z'
---

# Web Accessibility 2 - Focus

* Focus - Focus referes to the control on the computer screen that receives input from your keyboard and the clip board when you paste.
  * If you press the tab key you will then move on to the next option that has control.
  * All page functionality should be available by the keyboard, unless its something that you couldn't normally use a keyboard for.
* Pressing the tab key moves the focus forward, shift + tab moves focus backwards.
* Use the arrow keys to move focus around within a component.
  * Safari uses option+tab to move focus as default.
* Making sure your application has a logical tab order is an important step.
* Not all elements are focusable.
  * There's generally no need to focus on something if a user can't interact with it or provide some sort of input.

## DOM Order Matters

* Even if you change the visual order, this doesn't necessarily change the DOM order.
* Be careful when you use CSS to visually change the elemetns on screen.
  * For users relying on a keyboard, this can be exteremly frustrating.
  * WebAIM: 1.3.2 Meaningful Sequence - The reading and navigation order (determined by code order) is logical and intuitive.
  * General rule of thumb: TAB through the page every so often to make sure you haven't messed up the tab order.

## Using Tabindex

* Tabindex can be applied to a lot of objects, and is useful for modal windows, or dropdowns.
* Tabindex takes a range of values.
* `tabindex = "-1"`
  * Not in the natural tab order
  * Can be programatically focused with focus() method in javascript.
  * Useful for offscreen content
* `tabindex = "0"`
  * In the natural tab order
  * Can be programmatically focused.
* `tabindex = "5"`
  * In the natural tab order
  * Jumped to the front of the tab order.
  * Anti-pattern: This will potentially mess up the tab order and can be confusing for screen reader users.
  * If you need to put something earlier in the DOM Order, the best bet is to move it up in the DOM.

## Deciding whats in focus.

* Normally you only want to add focus to interactive controls.
* Is this something the user is going to interact with?

## Managing Focus

* For single page applications (ie React), You'll want to pick an approrpriate header, give it a taborder of -1 and then call its focus method after the user has taken their action.
  * This is a scenario that keeps the users focus along with the visual content.

## Skip Links

* Create a hidden link that allows keyboard and switch users to skip straight to the main content.

```html

<a href="#maincontent" class="skip-link"> Skip to main content</a>
<nav>
  ...
</nav>
<main id="maincontent" tabindex="-1">
  ...
</main>

```

```css

.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #3695d8;
  color: white;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}

```

## Focus in Complex Components

* The ARIA design pattern stock lists various components along with the various keyboard methods they support.

### Keyboard Design Patterns

* For example, with radio buttons. We can use roving focus:
  * Roving focus works by setting the tabindex to -1 for all the list children, then 0 for active item.
  * We then set tabindex to 0 for the next item, -1 for the previous method. We call the focus item and checked attribute to the new element.
  * When you reach the bottom of the list, we wrap back around to the top.

### Offscreen Content

* If, when tabbing through content, you see the focus disappear, you can use `document.activeElement` in the console to see which item has the focus.
* Chrome Accessibility Dev Tools.

### Modals and Keyboard Traps

* Keyboard traps are when tab doesn't move an element on.
* The checklist says:
  * 2.1.2 - No Keyboard Traps
    * Keyboard focus is never locked or trapped at one particular page element. The user can navigate to and from all navigable page elements using only a keyboard.
* For example, with modals we do want a temporary keyboard trap.

```javascript

var focusedElementBeforeModal;

var modal = document.querySelector('.modal');
var modalOverlay = document.querySelector('.modal-overlay');

var modalToggle = document.querySelector('.modal-toggle');
modalToggle.addEventListener('click', openModal);

function openModal() {
  focusedElementBeforeModal = document.activeElement;

  modal.addEventListener('keydown', trapTabKey);

  modalOverlay.addEventListener('click', closeModal);

  var signUpBtn = modal.querySelector('#signup');
  signUpBtn.addEventListener('click', closeModal);

  var focusableElementsString = 'a[href], area[href], input:not([disabled]), select:not([disabled]), textarea:not([disabled]), button:not([disabled]), iframe, object, embed, [tabindex="0"], [contenteditable]';
  var focusableElements = modal.querySelectorAll(focusableElementsString);

  focusableElements = Array.prototype.slice.call(focusableElements);

  var firstTabStop = focusableElements[0];
  var lastTabStop = focusableElements[focusableElements.length - 1];

  // Show the modal and overlay
  modal.style.display = 'block';
  modalOverlay.style.display = 'block';

  // Focus first child
  firstTabStop.focus();

  function trapTabKey(e) {

    // Check for TAB key press
    if (e.keyCode === 9) {

      // SHIFT + TAB
      if (e.shiftKey) {
        if (document.activeElement === firstTabStop) {
          e.preventDefault();
          lastTabStop.focus();
        }
      
      // TAB
      } else {
        if (document.activeElement === lastTabStop) {
          e.preventDefault();
          fiestTabStop.focus();
        }
      }
    }
    
    // ESCAPE

    if (e.keyCode === 27) {
      closeModal();
    }
  }
}

function closeModal() {
  // Hide the modal and overlay
  modal.style.display = 'none';
  modalOverlay.style.display = 'none';

  // Set the focus back to element that had it before the modal was opened.
  focusedElementBeforeModal.focus();
}

```





