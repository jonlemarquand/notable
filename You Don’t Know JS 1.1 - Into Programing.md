---
tags: [javascript, Notebooks, Notebooks/ydkjs]
title: You Don’t Know JS 1.1 - Into Programing
created: '2019-08-29T20:22:10.456Z'
modified: '2020-02-09T17:58:54.269Z'
---

# You Don’t Know JS 1.1 - Into Programing

# You Don’t Know JS 1.1 - Into Programing

A program, often referred to as source code or just code, is a set of special instructions to tell the computer what tasks to perform.

Usually code is saved in a text file.

The rules for formatting and combinations of instructions is called a computer language, sometimes referred to as syntax.

## Statements

A group of words, numbers, and operators that performs a specific task is a statement.

In javascript, a statement might look as follows:

`a = b  * 2;`

The characters a and b (in this example) are called variables.

The way to think about these is simple boxes you can store your stuff in.

In programs, variables hold values (like the number 42) to be used by the program.

Think of them as symbolic placeholders for the values themselves.

By contrast, the 2 is just a value itself, called a literal value, because it stands alone without being stored in a variable.

The = and * characters are operators.

They perform actions with the values and variables such as assignment and mathematic multiplication.
Most statements in JavaScript conclude with a semicolon (;) at the end.

## Expressions

Statements are made up of one or more expressions.

An expression is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

A general expression that stands alone is also called an expression statement, such as the following:

`B * 2;`

This expression statement is not very common or useful, as it wouldn’t have any effect on running the program.

It would retrieve the value of b and multiply it by 2, but then wouldn’t do anything with that result.

A more common expression statement is a call expression statement, as the entire statement is the function call expression itself:

`alert(a);`

## Executing a Program

These statements need to be executed, also referred to as running the program.

Statements like `a = b * 2` are helpful for developers when reading and writing, but are not actually in a form the computer can directly understand. 

A utility on the computer (either an interpreter or a compiler) is used to translate the code.

For some computer languages, this translation of commands is done from top to bottom, line by line, everytime the program is run.

This is known as interpreting the code.

For other languages, the translation is done ahead of time, called compiling the code, so when the program runs later, what’s running is actually the already compiled computer instructions ready to go.
The JavaScript engine actually compiles the program on the fly then immediately runs the compiled code.

## Output

What does console.log(..) actually do?

This is how we print text (aka output to the user) in the developer console.

First, the log( b ) part is a function call.

What’s happening is we’re handing the b variable to that function, which asks it to take the value of b and print it to the console.

Second, the console. Part is an object reference where the log(..) function is located.

## Input

The most common way to receive input is for the HTML page to show form elements (like text boxes) to a user that they can type into, and then using JS to read those values into your program’s variables.

An easy way for demonstration is to use the prompt(..) function:

```javascript
Age = prompt( "Please tell me your age:");
console.log( age );
```

## Operators

Operators are how we perform actions on variables and values.

The = equals operator is used for assignment

We calculate the value on the right-hand side (source value) of the = and then put it into the variable that we specify on the left-hand side (target variable).

Whilst not technically an operator, you’ll need the keyword var in every program, as it’s the primary way you declare (aka create) variables.

You should always declare the variable by name before you use it.

You only need to declare a variable once for each scope; it can be used as many times as needed.

### Common Operators in JavaScript

* Assignment: = as in a = 2.
* Math: + (addition), - (subtraction), * (multiplication), and / (division).
* Compound Assignment: +=, -=, *=, and /= are compound operators that combine a math operation with assignment 
  * as in a += 2 (same as a = a + 2).
* Increment/Decrement: ++ (increment), -- (decrement), as in a++ (similar to a = a + 1).
* Object Property Access: . as in console.log().
  * Objects are values that hold other values at specific named locations called properties. 
  * obj.a means an object value called obj with a property of the name a.
  * Properties can alternatively be accessed as obj["a"]
* Equality: == (loose-equals), === (strict-equals), != (loose not-equals), !== (strict not-equals), as in a == b.
* Comparison: < (less than), > (greater than), <= (less than or loose-equals), >= (greater than or loose-equals), as in a <= b.
* Logical: && (and), || (or), 
  * as in a || b that selects either a or b.

These operators are used to express compound conditionals (see "Conditionals"), like if either a or b is true.

## Values & Types

If you ask an employee at a phone shop how much a phone costs, and they say 99 quid, they’re giving you an actual numeric figure that represents what you’ll need to pay to buy it. If you want to buy two of the phones, you can easily to the mental maths and double the value to get the cost.

If the employee picks up the phone and says it’s free, they’re not giving you a number but instead another kind of representation of the expected cost (£0) - the word “free”.

When you later ask if the phone includes a charger, the answer could only have been either “yes” or “no".

These different representations for values are called types in programming terminology. JavaScript has built-in types for each of these so called primitive values:

* When you need to do maths, you want a number.
* When you need to print a value on the screen, you need a string
  * One or more characters, words, sentences
* When you need to make a decision in your program, you need a boolean.
  * True or false.

### Converting Between Types

If you have a number but need to print it on the screen, you need to convert the value to a string.
This conversion is known as “coercion”.

#### Explicit coercion:

```javascript
var a = “42”;
var b = Number(a);

console.log(a); // “42”
console.log(b); // 42
```
#### Implicit coercion:

When comparing the string “99.99” to the number 99.99, mose people would agree they are equivalent.
However, they are more likely “loosely equal”.

If you use the == loose equals operator to make the comparison, JavaScript will convert the left-hand side “99.99” to its number equivalent 99.99.

The comparison then becomes 99.99 == 99.99, which of course is true.

## Code Comments

The phone store employee might jot down some notes on the features of a newly released phone or on the new phone plans her company offers. These notes are for the employee, not for customers, but help the employee do their job better by documenting the hows and whys of what they should tell customers.

An ideal of coding is not just to make code that works correctly, but programs that make sense when examined.

Some observations and guidelines:

* Code without comments is suboptimal
* Too many comments (one per line, for example) is probably a sign of poorly written code.
* Comments should explain why, not what. They can optionally explain how if that’s particularly confusing.
* In JavaScript you can use:
```javascript
// This for a single line comment.
/* this for a multi-line comment */
```
## Variables

Most programs need to track a value as it changes over the course of the program, and the easiest way to do this is to assign the value a symbolic container, called a variable -- so called because the value in this container can vary over time as needed.

In some programming languages, you declare a value to hold a specific type of value, however, with JavaScript, we use dynamic typing.

Meaning variables can hold values of any type without any type enforcement.

See this example:
```javascript
var amount = 99.99;

amount = amount * 2;

console.log( amount );		// 199.98

// convert `amount` to a string, and
// add "$" on the beginning
amount = "$" + String( amount );

console.log( amount );		// "$199.98"
```
In this example the amount variable starts as a number, before being explicitly coerced into becoming a string.

Amount holds a running value that changes over the course of the program, illustrating the primary purpose of variables: managing program state.

State is tracking the changes to values as your program runs.

Another common example of using variables is for centralising value setting.

This is more typically called constants, when you declare a variable with a value and intend for that value to not change throughout the program.

You declare these constants, often at the top of a program, so that it’s convenient for you to have one place to go to alter a value if you need to.

By convention, JavaScript variables as constants are usually capitalised, with underscores _ between multiple words.

The newest version of JavaScript (ES6) includes a new way to declare constants, by using const instead of var:

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after initial setting.

If you tried to assign a different value to the const after the first declaration, your program would reject the change.

## Blocks

The phone store employee must go through a series of steps to complete the checkout as you buy your new phone.

Similarly, in code we often need to group a series of statements together, which we often call a block.

Unlike most other statements, a block statement does not need a semicolon (;) to conclude it.

In JavaScript, a block is defined by wrapping one or more statements inside a curly-brace { … } pair. 

Consider:
```javascript
var amount = 99.99;

// is amount big enough?
if (amount > 10) {			// <-- block attached to `if`
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

## Conditionals

“Do you want to add on the extra sceen protectors to your purchase for £9.99?”. The helpful phone store employee has asked you to make a decision, but first you need to consult the state of your wallet to answer the question.

There are quite a few ways we can express conditionals (aka decisions) in our programs.

The if statement is one of the most commons ways.

Essentially, you’re saying, “If this conditions is true, do the following…”. For example:
```javascript
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
	console.log( "I want to buy this phone!" );
}
```
The if statement requires an expression in between the parentheses ( .. ) that can be treated as either true or false.

You can also provide an alternative if the condition isn’t true, called an else clause. Consider:
```javascript
const ACCESSORY_PRICE = 9.99;

var bank_balance = 302.13;
var amount = 99.99;

amount = amount * 2;

// can we afford the extra purchase?
if ( amount < bank_balance ) {
	console.log( "I'll take the accessory!" );
	amount = amount + ACCESSORY_PRICE;
}
// otherwise:
else {
	console.log( "No, thanks." );
}
```
Here, if amount < bank_balance is true, we’ll print out “I’ll take the accessory!” and add the 9.99 to our amount variable.

Otherwise, the else clause says we’ll just politely respond “No thanks” and leave amount unchanged.

The if statement expects a boolean, but if you pass it something that’s not already boolean, coercion will occur.

JavaScript defines a list of specific values that are considered “falsy” because when coerced into a boolean, they become false.

These include “” and 0.

Any other value not on the “falsy” list is automatically “truthy” -- when coerced to a boolean they become true.

“Truthy” values include things like 99.99 and “free”.

Conditionals exist in other forms besides the if.

For example, the switch statement can be used as a shorthand for a series of if… else statements.

Loops use a conditional to determine if the loop should keep going or stop.

## Loops

During busy times, there’s a waiting list for customers who need to speak to the phone store employee. 
While there’s still people on that list, she just needs to keep serving the next customer.

Repeating a set of actions until a certain conditions fails -- in other words, repeating only while the condition holds -- is the job of programming loops.

A loop includes the test condition as well as a block (typically as { .. }).

Each time the loop block executes, that’s called an iteration.

For example, the while loop and the do..while loop forms illustrate the concept of repeating a block of statements until a condition no longer evaluates to true:

```javascript
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}

// versus:

do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```
The only practical difference between these loops is whether the conditional is tested before the first iteration (while) or after the first iteration (do..while).

In either form, if the conditional tests as false, the next iteration will not run.

That means if the condition is initially false, a while loop will never run, but a do..while loop will run just the first time.

The conditional is tested on each iteration, much as if there is an implied if statement inside the loop.

We can use JavaScript’s break statement to stop a loop.

Also, we can observe that it’s awfully easy to create a loop that would otherwise run forever without a breaking mechanism.
```javascript
var i = 0;

// a `while..true` loop would run forever, right?
while (true) {
	// stop the loop?
	if ((i <= 9) === false) {
		break;
	}

	console.log( i );
	i = i + 1;
}
// 0 1 2 3 4 5 6 7 8 9
```
While a while (or do..while) can accomplish the task manually, there’s another syntactic form called a for loop for just that purpose:
```javascript
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
```
As you can see, in both cases the conditional i <= 9 is true for the first 10 iterations (i of values 0 through 9) of either loop form, but becomes falseonce i is value 10.

The for loop has three clauses: 

* the initialization clause (var i=0), 
* the conditional test clause (i <= 9), 
* and the update clause (i = i + 1). 

So if you're going to do counting with your loop iterations, for is a more compact and often easier form to understand and write.

## Functions

The phone store employee probably doesn’t carry around a calculator to figure out the taxes and final purchase amount. That’s a task she needs to define once and reuse over and over again.

Odds are, the company has a checkout register with those “functions” built in.

Similarly, your program will almost certainly want to break up the code’s tasks into reusable pieces, instead of repeatedly repeating yourself repetitiously. The way to do this is to define a function.

A function is generally a named section of code that can be “called” by name, and the code inside it will be run each time. Consider:
```javascript
function printAmount() {
	console.log( amount.toFixed( 2 ) );
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
```
Functions can optionally take arguments (aka parameters) -- values you pass in. And they can also optionally return a value back.
```javascript
function printAmount(amt) {
	console.log( amt.toFixed( 2 ) );
}

function formatAmount() {
	return "$" + amount.toFixed( 2 );
}

var amount = 99.99;

printAmount( amount * 2 );		// "199.98"

amount = formatAmount();
console.log( amount );			// "$99.99"
 ```
The function printAmount(..) takes a parameter that we call amt. The function formatAmount() returns a value. 

Of course, you can also combine those two techniques in the same function.

Functions are often used for code that you plan to call multiple times, but they can also be useful just to organize related bits of code into named collections, even if you only plan to call them once.

Consider:
```javascript
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount( amount );

console.log( amount.toFixed( 2 ) );		// "107.99"
```
Although calculateFinalPurchaseAmount(..) is only called once, organizing its behavior into a separate named function makes the code that uses its logic (the amount = calculateFinal... statement) cleaner. 

If the function had more statements in it, the benefits would be even more pronounced.

## Scope

If you ask the phone store employee for a phone model that her store doesn’t carry, she will not be able to sell you the phone you want. She only has access to the phones in her store’s inventory. You’ll have to try another store to see if you can find the phone you’re looking for.

Programming has a term for this concept: scope.

Scope is basically a collection of variables as well as the rules for how those variables are accessed by name.

Only code inside that function can access that function’s scoped variables.

A variable name has to be unique within the same scope -- there can’t be two different a variables sitting right next to each other.

But the same variable name could appear in different scopes.
```javascript
function one() {
	// this `a` only belongs to the `one()` function
	var a = 1;
	console.log( a );
}

function two() {
	// this `a` only belongs to the `two()` function
	var a = 2;
	console.log( a );
}

one();		// 1
two();		// 2
```
A scope can also be nested inside another scope.

Like a clown at a birthday party who blows up one balloon inside another baloon.

If one scope is nested inside another, code inside the innermost scope can access variables from either scope.
```javascript
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// we can access both `a` and `b` here
		console.log( a + b );	// 3
	}

	inner();

	// we can only access `a` here
	console.log( a );			// 1
}

outer();
```

