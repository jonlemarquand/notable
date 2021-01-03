# How Javascript Works

## How Our Code is Exectured: JavaScript Parsers and Engines

* What happens is that a 'Javascript Engine', a program that exists to execute javascript code. This mostly happens in a browser.
    * Parser -- (Abstract Syntax Tree) --> Conversion to Machine Code -- (Machine Code) --> Code Runs

## Execution Context and the Execution Stack

* Execution Context: All javascript code needs to be run in an environment which is called an execution context.
    * A box, a container or a wrapper which stores variables and in which a piece of our code is evaluated and executed.
* The default is the Global Execution Context:
    * Code that is not inside any function.
    * Associated with the global object.
    * In the browser, that's the window object.
        * `lastName === window.lastName`
        * Everything that we declare in the global context gets assigned to the window object.
* Code in a function gets its own execution context.
* For a rundown of execution stack, run the following:

```javascript

var name = 'Jon';

function first() {
    var a = 'Hello! ';
    second();
    var x = a + name;
    console.log(x);
}

function second() {
    var b = 'Hi!';
    third();
    var y = b + name;
    console.log(y);

}

function third() {
    var c = 'Hey!';
    var z = c + name;
    console.log(z);

}

first();


```
* In this example the console would log `Hey! Jon` --> `Hi! Jon` --> `Hello! Jon` in that order due to the execution stack.

### Hoisting in Practice

* In the creation phase of the execution context, the function is stored in the variable function.
    * It is then read to use in the execution stage.
    * This only works for function declarations.
* Hoisting with functions doesn't work with function expressions.
    * i.e. `var retirement = function(year) {}` has to be before retirement(year) in the global execution context.
* With variables, they need to be declared before they can be use.
    * In the creation phase, the codee is scanned for variables and they are set to undefined.

```javascript

console.log(age);
var age = 23;

function foo() {
    var age = 65;
    console.log(age);
}
foo();
console.log(age);

// result: 65
// result: 23

```

* The variable is declared in the global execution context, and then the foo gets its own variable called age.
    * They are considered two different variables.

## Scoping and the Scope Chain

* Scoping answers the question, "where can we access a certain variable?"
* Each new function creates a scope.
    * The space/environment, in which the variables it defines are accessible.
* Lexical scoping: A function that is lexically within another function gets access to the scope of the outer function.
    * This does not work backwards unless the variables are returned as values.

### Execution Stack vs Scope Chain

* The execution stack determines the order in which scope is called but the scope chain is the order in which functions are written lexically.
* In the following example `c` will be logged as undefined because although the third function can be called by the second, it does not have the scope access to the second's variables.

```javascript

var a = "Hello!";
first();

function first() {
    var b = "Hi!";
    second();

    function second() {
        var c = 'Hey!';
        third()
    }
}

function third() {
    var d = 'Jon'
    console.log(c);
}

// Undefined

```

## The 'This' Keyword

* Every execution context gets this variable.
* Regular function call: The this keyword points at teh globnal object (the window object in the browser)
* Method call: The `this` variable points to the object that is calling the method.
* The `this` keyword is not assigned a value until a function where it is defined is actually called.
* In a regular function call, the this keyword is attached to the global object.
    * In function expressions, the `this` keyword will be applied to the object.
* Method borrowing: `mike.calculateAge = john.calculateAge`
    * This takes the function and replicates it for the mike object.
    * The this keyword will then apply to the mike object.