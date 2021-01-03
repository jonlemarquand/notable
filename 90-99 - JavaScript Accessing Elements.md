---
title: '90-99 - JavaScript: Accessing Elements'
created: '2019-12-06T15:08:59.197Z'
modified: '2019-12-06T15:09:00.990Z'
---

# 90-99 - JavaScript: Accessing Elements
* Classes and IDs (Especially IDs) can be used in javascript
	* IDs are unique so are best to use.

document.getElementById(“text”).innerHTML = “Hello Jon!”

* document - get something within the document we’re working
* .getElementbyId - get the element that we’re interested in by it’s ID
* (“text”)
* .innerHTML - gets the HTML from inside the ID
* = “Hello Jon!” - changes the text

*Comments within JavaScript*

// One line comment

/* This
Begins
a Multi
Line Comment */

*Create a Button to Change Text*

<p id=“text”>Hello World!</p>

<button id=“MyButton”>Change Text</button>

<script type=“text/javascript”>

document.getElementById(“MyButton”).onclick = function() {
alert(“Button Clicked”);
document.getElementById(“text”).innerHTML = “Hello Jon!”;
}

</script>

* Adding on click is similar to the in-line JavaScript used previously.
* When a function is defined we always put brackets after it.
	* Often the brackets will have variables in them if we are going to parse some variables.
 
 
*Create a Button to Append Text*

<p id=“text”>Javascript is… </p>

<button id=“secondButton”>Change Text</button>

<script type=“text/javascript”>

document.getElementById(“secondButton”).onclick = function() {
document.getElementById(“text”).innerHTML = document.getElementById(“text”).innerHTML + “awesome!";
}

</script>
* This takes the element in question (text) and appends something on the end when the button is pressed.

*Appending Styles*
 
 
Similarly to appending text, we can also append styles of the text/elements. To do this:

<p id=“text”>Javascript is red </p>

<button id=“thirdButton”>Change Text Colour</button>

<script type=“text/javascript”>

document.getElementById(“thirdButton”).onclick = function() {
document.getElementById(“text”) *.style.colour* *= “red"*
}

</script>

* By switching from .innerhtml to change the inner HTML to .style.colour this allows us to change the colour of the text.
* Can also use .style.fontSize (not font-size [no hyphens])
* .style.display = “none” will make an element disappear.
	* By setting the CSS display property to none. Removed from the flow of the page as well.
