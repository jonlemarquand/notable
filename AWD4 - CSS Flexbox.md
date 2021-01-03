---
tags: [css, css/flexbox, Notebooks, Notebooks/pd-advanced-web-developer]
title: AWD4 - CSS Flexbox
created: '2019-08-29T14:19:47.347Z'
modified: '2020-03-25T14:04:31.169Z'
---

# AWD4 - CSS Flexbox

## What is Flebox?
* It’s a more efficient way to lay out, align and distribute space among items in a container. (Even if their size is unknown)
* It makes it very easy to make responsive websites.
* Container Properties: flex-direction, justify-content, flex-wrap, align-items, align-content
* Flex Item Properties: order, flex, flex-grow, flex-shrink, align-self

```css
.container {
	display: flex;
}
```


## Basic Terminology
* *flex container* - whatever item has the display: flex property.
* *flex item* - if they are inside a container with display: flex, they are flex items.
* *main axis* - not always from left to right but this is the default
* *cross axis* - always the opposite to the main axis. (If main is x, cross is y)


## Flex Container Properties

### Flex-Direction

* Specifies how items are paced in the flex container.
	* Defining the *main axis* and its *direction*
* Default is `flex-direction: row;`
* `flex-direction: row-reverse;` makes it go right to left.
	* e.g. from 9 to 1 rather than 1 to 9.
* `flex-direction: column` makes the main axis go from top to bottom.
* `flex-direction: column-reverse` does 9 to 1 in a column.

### Flex-Wrap

* Specifies whether items are forced into a single line OR can be wrapped into multiple lines.
* `flex-wrap: wrap`
* `flex-wrap: wrap-reverse` changes the direction of the cross axis so that all subsequent rows will be above.

### Justify-content

* Defines how the space is distributed between items in flex container *along the main axis*
* `justify-content: flex-end;` - moves the content to the end of the flexbox along the main access.
* `justify-content: center;` - moves the content to the centre of the flex container with no spaces between them.
* `justify-content: space-between;` - takes the space and divides it between the elements but not on the edges.
* `justify-content: space-around;` - evenly divide the space around the elements
	* by default there’s double the space between elements because there is space on the left and right of each items.

### Align-items

* Defines how space is distributed between items in flex container *along the cross axis*
* `align-items: stretch`
	* This is the default value.
	* Stretches the content out along the cross axis to fill the space.
* `align-items: flex-start` - flush with the start of the cross axis
* `align-items: flex-end` - flush with the end of the cross axis
* `align-items: center` - centers them along the cross axis
* `align-items: baseline` - baseline allows us to align the content along the baseline of the text

### Align-content

* Defines how space is distributed between rows in flex container along the cross axis.
* For when content is wrapped.


## Flex Item Properties
### Align-self

* Allows you to override align-items on individual flex items along the cross-axis.

### Order

* Specifies the order used to lay out items in their flex container.
* Write them one way in the HTML but then reorder them using the CSS.
	* Useful for responsive layouts.
* All items by default have an order of 0.
	* Therefore if you set `.box-3 {order: 2;}` it would simply go to the end of the row rather than just reshuffle.
	* You can use negative numbers to jump items to the front.

### flex-basis

* What if you want a layout with boxes of different sizes?
* flex-basis is like width, but not.
	* like height when you’re working with columns.
	* flex-basis takes priority over width
* Specifies the ideal size of a flex item *before* it’s placed into a flex container.

### flex-grow

* Dictates how the unused space should be spread amongst flex items.
	* It’s all about ratios.
* To make all boxes share space evenly:

```css
.box {
	flex-grow: 1;
}
```

* It means use twice as much space as the other boxes get rather than double the size:

```css
.box {
	flex-grow: 1;
}

.box-1 {
	flex-grow: 2;
}
```

### flex-shrink

* Dictates how items should shrink when there isn’t enough space in container.
* Uses a different formula for how to distribute the space than flex-grow does.

## Browser Support for Flexbox
* Fully supported across all modern browsers and in IE11 there might be some bugs.
