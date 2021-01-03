---
tags: [javascript, ydkjs]
title: You Don't Know JS 1.2 - Into Javascript
created: '2019-08-29T22:10:31.447Z'
modified: '2019-09-14T11:03:56.016Z'
---

# You Don't Know JS 1.2 - Into Javascript

## Values & Types

Javascript provides a `typeof` operator that can examine a value and tell you what type it is.

```javascript
var a;
typeof a;				// "undefined"

a = "hello world";
typeof a;				// "string"

a = 42;
typeof a;				// "number"

a = true;
typeof a;				// "boolean"

a = null;
typeof a;				// "object" -- weird, bug

a = undefined;
typeof a;				// "undefined"

a = { b: "c" };
typeof a;				// "object"
```
* Despite appearances, `typeof a` is describing 'type of the value currently in a' rather than 'type of a'.
  * This is because only values have types in Javascript; variables are just containers for these values.
* `typeof null` is an interesting case, because it errantly returns "object", when you'd expect it to return "null".
  * Warning: This is a long-standing bug in JS, but one that is likely never going to be fixed. Too much code on the Web relies on the bug and thus fixing it would cause a lot more bugs!

### Objects

* The object type refers to a compound value where you can set properties that each hold their own properties of any type.

```javascript

var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true

```
* Properites can be accessed in two ways:
  * **Dot Notation** i.e. `obj.a`
  * **Bracket Notation** i.e. `obj.["a"]`
* Dot notation is shorter and generally easier to read.
* Bracket notation is useful if you have a property name that has special characters in it, like `obj["hello world!"]` 
  * Such properties are often referred to as keys when accessed via bracket notation. 
  * The [ ] notation requires either a variable (explained next) or a string literal (which needs to be wrapped in " .. " or ' .. ').

### Arrays

* An array is an object that holds values (of any type) not particulary in named properties or keys but in numerically indexed positions.
* For example:

```javascript
var arr = [
"hello world",
42,
true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"
```
* Because arrays are special objects, they can also have properties assigned to them.
  * For example, the automatically updated length property.

### Functions

```javascript
function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo;			// "function"
typeof foo();		// "number"
typeof foo.bar;		// "string"
```

* Functions are a subtype of objects.
* `typeof` returns "function" which implies function is a main type.
  * However, you typically only use function properties in limited cases.

### Built-In Type Methods

The built-in types and subtypes we've just discussed have behaviors exposed as properties and methods that are quite powerful and useful.

For example:

```javascript
var a = "hello world";
var b = 3.14159;

a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4);			// "3.1416"
```
## Comparing Values

* There are two main types of value comparison that you will need to make in your JS programs: *equality* and *inequality*.
* The result of any comparison is a strictly boolean value (`true` or `false`), regardless of what types are compared.

### Coercion

* Coercion comes in two forms in Javascript, *explicit* and *implicit*.
  * Explicit coercion is simply that you can see obviously from the code that a conversion from one type to another will occur.
  * Implicit coercion is when the type conversion can happen as more of a non-obvious side effect of some other operation.

### Truthy & Falsy

The specific list of "falsy" values in JavaScript is as follows:

* "" (empty string)
* 0, -0, NaN (invalid number)
* null, undefined
* false

Any value that's not on this "falsy" list is "truthy." Here are some examples of those:

* "hello"
* 42
* true
* [ ], [ 1, "2", 3 ] (arrays)
* { }, { a: 42 } (objects)
* function foo() { .. } (functions)

It's important to remember that a non-boolean value only follows this "truthy"/"falsy" coercion if it's actually coerced to a boolean.

### Equality

There are four equality operators: `==`, `===`, `!=`, and `!==`. The `!` forms are of course the symmetric "not equal" versions of their counterparts; non-equality should not be confused with inequality.

The difference between `==` and `===` is usually characterized that `==` checks for value equality and === checks for both value and type equality. However, this is inaccurate. The proper way to characterize them is that `==` checks for value equality with coercion allowed, and `===` checks for value equality without allowing coercion; `===` is often called "strict equality" for this reason.

Consider the implicit coercion that's allowed by the == loose-equality comparison and not allowed with the === strict-equality:

```javascript
var a = "42";
var b = 42;

a == b;			// true
a === b;		// false
```

In the `a == b` comparison, JS notices that the types do not match, so it goes through an ordered series of steps to coerce one or both values to a different type until the types match, where then a simple value equality can be checked.

To boil down a whole lot of details to a few simple takeaways, and help you know whether to use `==` or `===` in various situations, here are my simple rules:

* If either value (aka side) in a comparison could be the true or false value, avoid `==` and use `===`.
* If either value in a comparison could be one of these specific values (`0`, `""`, or `[]` -- empty array), avoid `==` and use `===`.
* In all other cases, you're safe to use `==`. Not only is it safe, but in many cases it simplifies your code in a way that improves readability.

All these rules apply symetrically for the non-equality comparisons.

#### Inequality

* The `<`,`>`,`<=` and `>=` are typically used to compare numbers '3 < 4' is fairly straightforward to understand.
* However, Javascript `string` values can also be compared for inequality, using typical alphabetic rules `("bar" < "foo")`

## Variables

* In Javascript, variable names (including function names) must be valid identifiers.
  * An identifier must startt with `a - z`, `A - Z`, `$`, or `_`.
  * It can then contain any of those characters plus the numerals `0 - 9`.

### Function Scopes

You can use the `var` keyword to declare a variable that will belong to the current function scope, or the global scope if at the top level outside of any function.

#### Hoisting

* Whenever a `var` appears inside a scope, that delcaration is taken to belong to the entire scope and accessible everywhere throughout.
  * Metaphorically, this behaviour is called *hoisting*, when a `var` declaration is conceptually "moved" to the top of its enclosing scope.

```javascript
  var a = 2;
  foo();					// works because `foo()`
						// declaration is "hoisted"

  function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// declaration is "hoisted"
						// to the top of `foo()`
  }

  console.log( a );	// 2
```

* Warning: It's not common or a good idea to rely on variable hoisting to use a variable earlier in its scope than its `var` declaration.
  * More common to do this with function declarations.

#### Nested Scopes

* When you declare a variable, it is available anywhere in that scope, as well as any lower/inner scopes.
* If you try to access a variable's value in a scope where it is not available, you'll get a `ReferenceError`.
* In addition to creating declarations for variables at the function level, ES6 lets you declare variables to belong to individual blocks (pairs of { .. }), using the let keyword. Besides some nuanced details, the scoping rules will behave roughly the same as we just saw with functions:

```javascript
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```

* Because of using let instead of var, b will belong only to the if statement and thus not to the whole foo() function's scope. 
  * Similarly, c belongs only to the while loop. 
  * Block scoping is very useful for managing your variable scopes in a more fine-grained fashion, which can make your code much easier to maintain over time.

## Conditionals

In addition to the `if` statement Javascript provides a few other conditionals mechanisms that we should take a look at.

Sometimes you may find yourself writing a series of if..else..if statements like this:

```javascript
if (a == 2) {
	// do something
}
else if (a == 10) {
	// do another thing
}
else if (a == 42) {
	// do yet another thing
}
else {
	// fallback to here
}
```

This structure works, but it's a little verbose because you need to specify the a test for each case. Here's another option, the switch statement:

```javascript
switch (a) {
	case 2:
		// do something
		break;
	case 10:
		// do another thing
		break;
	case 42:
		// do yet another thing
		break;
	default:
		// fallback to here
}
```

The break is important if you want only the statement(s) in one case to run. If you omit break from a case, and that case matches or runs, execution will continue with the next case's statements regardless of that case matching. This so called "fall through" is sometimes useful/desired:

```javascript
switch (a) {
	case 2:
	case 10:
		// some cool stuff
		break;
	case 42:
		// other stuff
		break;
	default:
		// fallback
}
```

Here, if a is either 2 or 10, it will execute the "some cool stuff" code statements.

Another form of conditional in JavaScript is the "conditional operator," often called the "ternary operator." It's like a more concise form of a single if..else statement, such as:
```javascript
var a = 42;

var b = (a > 41) ? "hello" : "world";

// similar to:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

If the test expression (a > 41 here) evaluates as true, the first clause ("hello") results, otherwise the second clause ("world") results, and whatever the result is then gets assigned to b.

The conditional operator doesn't have to be used in an assignment, but that's definitely the most common usage.



