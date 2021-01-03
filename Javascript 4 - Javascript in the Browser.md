# Javascript 4 - Javascript in the Browser

## The DOM and DOM Manipulation

* DOM: Document Object Model
    * It is a structured representation of an HTML document.
    * The DOM is used to connect webpages to scripts like Javascript.
* For each HTML box, there is an object in the DOM that we can access and interact with.
* `document.querySelector()` - You can select things as you would with CSS. 
    * However, this only returns the first thing with the selector value.
    * On pages, you should only have one id anyway.
* To change text:
    * `document.querySelector('#current-0').textContent = dice;`
    * This can only set plain text, no HTML.
* If we want to set HTML as well use:
    * `document.querySelector('#current-0').innerHTML = '<em>' + dice + </em>`
* You could also use:
    `var x = document.querySelector('#score-0').textContent;`
* To change the css:
    * `document.querySelector('.dice').style.display = 'none'`;
    * This changes the inline css style.

## Events and Event Handling

* Events: Notifications that are sdent to notify the code that something happened on the webpage.
    * Clicking a button, resizing a window etc.
* Event listener: A function that performs an action based on a certain event. It waits for a specific event to happen.
* An event can only be processed once all the functions have returned.
* There is also the message queue in javascript where events are queued once they are called.
* `document.querySelector('.btn-roll').addEventListener('click', btn);`
    * This `.addEventListener` has an event type (common ones include click and load)
    * https://developer.mozilla.org/en-US/docs/Web/Events
    * Also has a function. In this example it's `btn` but could be function(){} if it doesn't need to be referenced again.
* `document.getElementById('score-0')` could also be used to select an id. 
    * It's faster but only works for ids.
    * Note you don't use the css selector '#score-0' but just the name.
* With a function, the () afterwards calls the function immediately.
    * Using `.addEventListener('click', init)` for example, will only run init when the event is triggered.

## Add and Change HTML Classes

* `document.querySelector('.player-0-panel').classList.remove('active');`
    * This removes the class.
* `document.querySelector('.player-1-panel').classList.add('active');`
    * Adds the class.
* Use `.classList.toggle('active');` to toggle between adding if it isn't there and removing if it is.

## The DRY (Don't Repeat Yourself) Principle

* Bad practice to have the exact same code in two places.
    * If you change something in one, you may forget to change it in another.
    * Better to create a new function in the highest scope possible so it can be used in multiple places.

## State Variables

* Tells us the condition of a system.
* 'Is our game playing, or is it not playing?'
    * Can use a boolean of `gamePlaying = true` and wrap code in event listeners in if statement.