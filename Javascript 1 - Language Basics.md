# Javascript 1 - Language Basics

## How to Add Scrips

### Inline

```html
</body>
<script>
    console.log('Hello World!')
</script>
```

### Outside

```html

<script src="script.js"></script>

```

## A Brief Introduction to Javascript

* Javascript is a lightweight, cross-platform, object-orientated computer programming language.
* Traditionally, javascript was only used in the browser environment. However, today javascript is used in different places:
    * Client-side: Javascript in the browser.
    * Server-side: Thanks to node.js, we can use JavaScript on the server as well.
* Javascript is what made modern web development possible.
    * Dynamic effects and interactivity.
    * Modern apps are also built with it.
* HTML, CSS and Javascript
    * HTML is the noun
    * CSS is the adjective
    * Javascript is the verb

## Variables and Data Types

* A variable is like a container in which we can store a value.
    * We can then use this code over and over.
    * When we declare text, generally just use single quotes.

### Data Types

* The data types:
    * Number: Floating point numbers, for decimals and integers.
    * String: Sequence of characters, used for text.
    * Boolean: Logical data type that can only be `true` or `false`
    * Undefined: Data type of a variable that does not have a value yet.
    * Null: Also means 'non-existent'.
* Javascript has dynamic typing: 
    * data types are automatically assigned to variables.

* Best practice is to give variables meaningful names rather than a sequence of characters.
    * Also convention to have wordSecondWord
* Variables can't start with numbers or symbols other than the `$` or `_`
* Also can't use javascript keywords.
    * `var function` will not work.

## Variable Mutation and Type Coercion

* Sidenote: Comments
    * We can use comments by typing `//` and then any comment we want for single line comments.
    * For multi-line comments use: `/*` and then `*/`

* Type Coercion - Javascript will automatically convert some variables to work in a string. For example:

```javascript

var firstName = 'Jon';
var age = 28;

console.log(firstName + ' ' + age);

```
* The number will converted into a string so that it can be combined with the other string.
* Both booleans and undefined will be converted into a string if you attempt to console.log it.

### Variable Mutation

* If after defining `age = 28`, if you then say `age = twenty eight` it will convert the variable to a string from a number.


## Basic Operators

* Operators are like functions that are written in a specific way.
* Math Operators:
    * `-` Minus - `2018 - 33` will produce the answer
    * `+` 
    * `*` - Multiply
    * `/` - Divide
* Logical Operators:
    * `>` - Greater than
        * var whoOlder = `33 > 28` Will give a boolean of true.
* typeof Operator:
    * typeof whoOlder - will return `boolean`
    * It finds out what kind of data type the operator is.

### Operator Precedence

* Which operator is operated first?
    * The maths operators are operated before the logical ones.
        * Within them * and / will work before + and -.
        * Therefore they need to be in parenthesis to work.
    * Example `var isFullAge = now - yearJon >= fullAge;` will return true.
* Rather than write `x = x * 2;` we can write `x *= 2`.
    * This can be done for the all maths operators.
* Rather than writing `x = x + 1;` we can write `x++`
    * This also works with `--`


## If/Else Statements

```javascript

var firstName = 'John';
var civilStatus = 'single';

if (civilStatus === 'married') {
    console.log(firstName + ' is married!');
} else {
    console.log(firstName + ' is very much single');
}

```

* If Statements need to be written as:
    * `if (civilStats === 'married') {}`
        * This has to be a boolean.
        * Could also be written as `var isMarried = true` and then `if (isMarried)`

## Boolean Logic

* AND `&&` => true if ALL are true.
* OR `||` => true if ONE is true.
* NOT `!` => inverts true/false value.

## The Ternary Operator and Switch Statements

* Ternary Operator: If/Else statement on one line
    * `age >=18 ? console.log(firstName+ ' drinks beer.') : console.log(firstName + ' drinks juice.')`
* Or `var drink = age >= 18 ? 'beer' : 'juice';`
* A shorter way of writing if/else.
* Switch Statements:

```javascript

var job = 'teacher';
switch (job) {
    case 'teacher':
    case 'instructor':
        console.log(firstName + ' teaches kids how to code.');
        break;
    case 'driver':
        console.log(firstName + ' drives an uber in Lisbon.');
        break;
    case 'designer':
        console.log(firstName + 'designs beautiful websites.');
        break;
    default:
        console.log(firstName + ' does something else.');
}

```
* This checks a variable and logs it.

## Truthy and Falsey Values and equality Operators

* Falsy Values: Undefined, null, 0, '', NaN
    * A value that is considered false when evaluated in an if/else statement.
* Truthy values:
    * A value that is considered true in an if/else statement
* Quick example: `height = 0; if (height || height === 0)`
    * Because 0 is a falsey value, you need an or statement.
* `==` does type coercion, `===` is used for string comparisons.

## Functions

* Functions are like containers that contain a piece of code that can be used over and over again.
* Syntax: `function nameofFunction(variableForUse) { return variableforUse - change; } nameofFunction(1999);`
    * Variable can also be called argument.
    * When you hit the return statement, it returns the value and immediately finishes the function.
* Dry principle: Don't repeat yourself, instead write a function.
* Functions return something if you want to use the value later, but can just do something (such as console.log)

### Function Statements and Expressions

* Writing a function in a slightly different way.
* A function expression:
    *  `var whatDoYouDo = function (job,firstName){}`
* A function declaration:
    * `function whatDoYouDo(job, firstName) {}`
* Javascript expressions are pieces of code that always produce a value.
    * It doesn't matter how long it is, as long as it is a single value.
    * Whenever javascript expects a value, we always have to write an expression.
* Statements, they perform actions but do not produce immediate values.

## Arrays

* Arrays are like collections of variables that can have different datatypes.
* `var names = ['John', 'Mark', 'Jane'];`
* Arrays are 0 based. They start from 0.
    * You can return the number of items in the array using `array.length`.
* You can replace the data by using `names[1] = 'Ben';` or add data to the array with `names[5] = 'Mary'`.
    * To add as the last element in the array use `names[names.length] = 'Mary';`
    * We can do something like `names.push('David');` which will also add an element to the array at the end.
* Other array methods:
    * `array.unshift` - Adds to the start of the array.
    * `array.pop()` - Removes the element from the end.
    * `array.shift()` - Removes the element from the start.
    * `array.indexOf()` - Returns the index value of the array.


## Objects

### Objects and Properties

* Each value has a name that is the key.
* We can group together values that have no particular order.
    * In objects, the order doesn't matter.
* The object literal:

```javascript

var jon = {
    firstName: 'Jon',
    lastName: 'Le Marquand',
    family: ['Ralph', 'Squish']
}

```
* This `firstName: 'Jon'` is known as a key-object pair.
* Arrays/objects can have arrays in them.
* To access them, we just have to use object.key
    * Or we can also use: `jon['lastName']`
* To create new objects:

```javascript

var jane = new Object();
jane.firstName = 'Jane';
jane['lastName'] = 'Smith';

```

### Objects and Methods

* We can attach functions to objects, and these are called methods.
* For example, we could store birthYear and age, but age would change every year.
    * So therefore in the object:

```javascript

var jon = {
    firstName: 'Jon',
    lastName: 'Le Marquand',
    family: ['Ralph', 'Squish']
    calcAge: function() {
        this.age 2018 - this.birthYear;
    }
}

jon.calcAge();
console.log(jon);

```

* In objects, we can use `this.` which means the present/current object.

## Loops and Iteration

* Rather than writing ten lines of the same code, we can automate it using a loop.
* ` for (inital counter value, condition evaluation, counter update)`
    * Initial value: i = 0;
    * Conditional evaluation: i < 10;
    * Counter update: i++
* `i` is the usual standard for a counter variable
* How this works:

```javascript

for (var i=0; i < 10; i++) {
    console.log(i);
}

// i = 0, 0 < 10 is true, log i to console, add 1 to i
// i = 1, 1 < 10 is true, log i to console, add 1 to i
// ...
// i = 9, 9 < 10 is true, log i to console, add 1 to i
// i = 10, 10 < 10 is false, exit the loop!

```
* A more practical example:

```javascript

var jon = ['Jon', 'Le Marquand', 1991, 'developer', false];
for (var i=0; i < jon.length, i++ {
    console.log(jon[i]);
}

```

* While Loops:
    * `var i=0; while(i < john.length) {console.log(john[i]); i++;}`

### Continue and Break Statements

* Continue: We use it to quit the current iteration of the loop and go on to the next one.
    * E.g. We only want to loop strings, so if it isn't quit the loop and continue with next iteration.

```javascript

var jon = ['Jon', 'Le Marquand', 1991, 'developer', false];
for (var i=0; i < jon.length, i++ {
    if (typeof jon[i] !== 'string') continue;
    console.log(jon[i]);
}

```
* Break: exits the current iteration and the entire loop.
