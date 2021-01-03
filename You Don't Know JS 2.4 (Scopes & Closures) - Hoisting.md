---
title: You Don't Know JS 2.4 (Scopes & Closures) - Hoisting
created: '2020-01-28T07:50:40.770Z'
modified: '2020-01-28T20:00:50.191Z'
---

# You Don't Know JS 2.4 (Scopes &amp; Closures) - Hoisting

## Chicken or the Egg?

* We can often think of code running straight through, line by line, as the program executes. 
* However, consider the following code:

```javascript

a = 2;
var a;
console.log(a);

```

* In this example, many would assume that it would come out `undefined`, however the answer would be 2. Why?
* It all depends on what comes first, the declaration ("egg"), or the assignment ("chicken")?

## The Compiler Strikes Again

* The engine will actually compile the javascript code before it interprets it.
* `var a = 2` seems like one statement but JavaScript actually thinks of it as two statements: `var a` and `a = 2`.
  * The first statement, the declaration, is processed during the compilation phase.
  * The second statement, the assignment, is left in place for the execution phase.
* One way of thinking is that this variable and function declarations are "moved" from where they appear in the flow of code to the top of the code.
  * This gives the name "Hoisting".
  * In other words, the egg (declaration) comes before the chicken (assignment)

## Functions First

* One thing that's important to note is that the hoisting is *per-scope*
* Both function declarations and variable declarations are hoisted. But a subtle detail is that functions are hoisted first, and then variables.
* In the following example, 1 is printed instead of 2:

```javascript

foo(); // 1

var foo;

function foo() {
  console.log(1);
}

foo = function() {
  console.log(2);
};

```

* Notice that `var foo` was the duplicate (and thus ignored) declaration, even though it came before the `function foo()` declaration, because function declarations are hoisted before normal variables.

