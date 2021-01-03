---
title: Javascript 6 - The Budget App Project
created: '2020-03-14T09:45:06.198Z'
modified: '2020-03-16T09:21:01.151Z'
---

# Javascript 6 - The Budget App Project

## Project Planning and Architecture: Step 1

* It's important to work out and have a plan for how we want data to flow with javascript.

### Structuing our Code with Modules

* Modules
  * Important aspect of any robust application's architecture
  * Keeps the units of code for a project both cleanly separated and organised
  * Encapsulate some data into privacy and expose other data publicly.
* Some tasks are to do with the UI and some Data and some are the controller module.

### Implementing the Module Pattern

* We create modules because we keep pieces of code that are related together.
  * Inside of these indepenent modules, so that no other code can override that variables.
  * We will also only expose a public interface, aka an API.
* How to write modules:
  ` var budgetController = (function() {})();`
  * An IFFE allows us to have data privacy because it has a new scope that is not visible to the outside.

  ## Setting Up the First Event Listeners

  ### Inputting Keyboard Events

  ```javascript
  
  document.addEventListener('keypress', function(event) {

    if (event.keyCode === 13 || event.which === 13) {
      console.log('ENTER was pressed')
    }

  });
  
  ```

* This is how we say 'do something when a key is pressed.'
* The `keyCode` is how we identify which key was pressed.
* `event.which` is for older browsers that don't recognise `event.keyCode`

### Reading Input Data

```javascript

return {
    getInput: function() {
      return {
        type: document.querySelector('.add__type').value, // Will be either inc or exp
        description: document.querySelector('.add__description').value,
        value: document.querySelector('.add__value').value
      };
    }
}

var input = UICtrl.getInput();

```

* This function will return and be usable in the globalController to recall data from the page.

### Data Structures

```javascript

// Bad data structures
var allExpenses = [];
var allIncomes = [];
var totalExpenses = 0;

// Good data structure
var data = {
  allItems: {
    exp: [];
    inc: []
  },
  totals: {
    exp: 0,
    inc: 0
  }
}

```
## Manipulating the DOM

* When adding HTML it's useful to have double quotes in the HTML and single quotes in Javascript or vice versa.



```javascript

// Create HTML string with place holder text

addListItem: function(obj,type) {

  var html, newHtml;

  if (type === 'inc') {
    html = '<div class="item__value">%value%</div>'
  } else if (type === 'exp') {
    html = '<div class="item__value">%expvalue%</div>
  }


// Replace the placeholder text with some actual data

  newHtml = html.replace('%value%', obj.id);
  newHtml = newHtml.replace('%description%', obj.description)

// Insert the HTML into the DOM

  var expensesList = document.getElementById(expenses_list)
  expensesList.insertAdjacentHTML('beforeend', newHtml);

},
```
### Clearing Our Input Fields

* Lesson Objectives:
  * How to clear HTML Fields
  * How to use querySelectorAll
  * How to convert a list to an array
  * Foreach
    * Pass a callback function into the method and then the callback function is applied to each element in the array.

```javascript
var fields, fieldsArr;

fields = document.querySelectorAll(DOMstrings.inputDescription + ', ' + DOMstrings.inputValue);

fieldsArr = Array.prototype.slice.call(fields);

fieldsArr.forEach(function(current, index, array) {
  current.value = "";
});

```

## Updating the Budget

### Updating the Budget: Controller

* How to convert field inputs to numbers
  * `parseFloat("202")` will convert a string to a number and we can use it to make calculations.
* How to prevent false inputs
  * Wrap code in an if statement with a `if (input.description !== "" && !isNan(input.value) && input.value > 0)`
  * This just means that the rest of the function will not run.
* A good philosophy is for functions to have one specific task.

### Updating the Budget: Budget Controller

* How and why to create simple, reusable functions with only one purpose
  * I.e. functions that set data, functions that return data. etc.
* How to sum all elements of an array using the forEach method:

```javascript

var calculateTotal = function(type) {
  var sum = 0;
  data.allItems[type].forEach(function(cur) {
    sum += cur.value;
  });
  data.totals[type] = sum;
};

```
* When there is no percentage to calculate, we use `-1` as the value.

## Event Delegation

* When the event is triggered, it is also triggered on all the parent elements as well.
  * We say the event bubbles up inside the DOM tree.
  * The first element is known as the target element. It is stored as a property in the event element.
* If we know where the event is triggered, we can attach an event handler to a parent element and wait for the event to bubble up. It will then do whatever we want to do with the target element.
  * This is known as event delegation.
* Use cases for event delegation:
  * When we have an element with lots of child elements that we are interested in. (Such as deleting an item from a list)
  * When we want an event handler attached to an element that is not yet int he DOM when our page is loaded.

## Deleting an Item

### Setting up the Delete Event Listener using Event Delegation

* How to use event delegation in practice.

```javascript

document.querySelector('.container').addEventListener('click', ctrlDeleteItem);
var ctrlDeleteItem = function(event) {
  var itemID, splitID;

  itemID = event.target.parentNode.parentNode.parentNode.parentNode.id);

  if (itemID) {
    splitID = itemID.split('-');
  }
};

```

* How to use IDs in HTML to connect the UI with the data model
  * The ID in the example project, have both the type and the id of the element.
* How to use the parentNode property for DOM traversing.
  * if we use the `parentNode` selector, it will select the parent selector of the HTML element.
* `.split` allows us to split a string at a specific character and put it into an array.

### Deleting an Item from our Budget Controller

* Map method - Yet another method to loop over an array.
  * `var ids data.allItems[type].map(function(current) {return current.id;});`
  * Map returns a brand new array.
  * For each element, it will store the thing in a new array.
* How to remove elements from an array using the splice method.
  * Splice removes elements from an array.
  * `data.allItems[type].splice(positionOfDeleteStart, number of items to delete)`

### Removing an Element from the DOM

* Remove child method:
  * We move up in the DOM so we can remove the child.
  * In javascript you can only delete a child element.
  * `document.getElementByID(selectorID).parentNode.removeChild(document.getElementByID(selectorID));`
  * `var el = document.getElementById(selectorID); el.parentNode.removeChild(el);`


## String Manipulation

```javascript

num = Math.abs(num); // Removes any - from before the number (returns an absolute number)
num = num.toFixed(2); // Number to 2 decimal places. Returns a string.

numSplit == num.split('.') // This will divide the number into the integer part and the decimal part

int = numSplit[0];

if (int.length > 3) {
  int = int.substr(0, int.length - 3) + ',' + int.substr(int.length - 3, int.length); // Position 0, read 1 element.
}

dec = numSplit[1];

return (type === 'exp' ? '-' : '+') + ' ' + int + dec;

```

## Displaying the Current Month and Year

* How to get the current date by using the date object constructor.
```javascript

displayMonth: function() {

  var now = new Date();
  // var christmas = new Date(2016, 11, 25);

},

```

We can then use methods to see when this `now` variable was created.

## How and When to use Change Events

* The change event occurs when a value is changed.





