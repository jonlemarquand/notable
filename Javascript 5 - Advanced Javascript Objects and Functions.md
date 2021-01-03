# Javascript 5 - Advanced Javascript: Objects and Functions

## Everything is an Object: Inheritance and the Prototype Chain

* Javascript saying: Everything is an object.
    * Primitives: Numbers, strings, booleans, undefined, null
    * Everything else is an object: Arrays, functions, objects, dates, wrappers for numbers, strings, booleans
* Object-Oriented Programming
    * objects interacting with one another through methods and properties.
    * Used to store data, structure applciations into modules and keeping code clean.
* We can make a constructor which acts like a blueprint and from there we create instances which can use all the objects.
* Inheritance - when one object is based on another object.
    * It gets access to another objects properties and methods.
    * Inheritance works by using prototype.
        * Every javascript object has a prototype property.
* Each and every object we create is created by an overall object prototype.
* The constructor's prototype property is NOT the prototype of the constructor itself, it's the prototype of ALL instances that are created through it.
* When a certain method (or property) is called, the search starts in the object itself, and if it cannot be found, the search moves on to the object's prototype. This continues until the method is found.
    * This is what's known as the prototype chain.

## Creating Objects: Function Constructors

* A function constructor is a pattern for writing a blueprint.
* Convention is that function constructors are always written with a capital letter.

```javascript

var Person = function(name, yearOfBirth, job) {
    this.name = name;
    this.yearOfBirth = yearofBirth;
    this.job = job;
}

var Jon = new Person ('Jon', 1991, Developer);

```
* This is called instanciation. They are instances of the constructor.
* When we use the new operator:
    * AN empty object is created.
    * The constructor function is called with the arguments we specify.
    * Calling a function creates a new execution variable with a this function.
    * The new operator makes it so the this function points to the new variable rather than the global object.
* `person.prototype.calculateAge = function(){console.log(2016-this.yearOfBirth)};`
    * This would mean that the function wouldn't be attached to the constructor but we can still use it because it is in the prototype property.

### The Prototype Chain in the Console

* In the console you can see the properties of the object.
* You can also see the `_proto_` which shows the things added by or inherited fromthe prototype.
* If you use `console.info(x);` you can dig down in to the properties of arrrays.
    * Length is stored as a property in the array.
    * You can also see the prototype of the array which is where you will find the methods.
    * All the arrays will have access to those methods because they are stored in the array prototype.

### Creating Objects: Object.create

```javascript

var personProto = {
    calculateAge: function() {
        console.log(2016 - this.yearofBirth);
    }
};


// Old Way
var john = Object.create(personProto);
john.name = 'John';
john.yearOfBirth = '1990';

// New Object.create
var jane = Object.create(personProto,
{
    name: { value: 'Jane' },
    yearOfBirth: { value: 1969 },
})

```

* Both share the same prototype
* The difference between them is that object.create builds an object that inherits directly from the one that we passed.
    * On the other hand, the funciton consructor, the newly createed object inherits directly from the functions prototype.
* Objects.create allows us to implement really complex inheritance structures than function constructors.
    * Because it allows us to specify which properties can be prototypes.

### Primitives vs Objects

* We know that ______ are primitives.
* Variables containing primitives actually hold that data inside of the variable itself.
    * On objects, variables assocaited with objects contain a reference to the place in memory where the object sits.

```javascript

var a = 23;
var b = a;
a = 46;
console.log(a); // 46
console.log(b); // 23


```
* This shows that each of the variables hold their own copy of the data.
* Two variables holding two primitives are two different things.

```javascript

var obj1 = {
    name: 'John',
    age: 26;
}
var obj2 = obj1;
obj1.age = 30;
console.log(obj1.age); // 30
console.log(obj2.age); // 30

```
* When we set object 2 to object 1, we just created a reference to object 1 which links to the same place in the memory.
    * That's why when we update the age for one, we also update the age for two.
* With functions, when we pass a primitive the function creates a copy so the primitive on the outside does not change.
    * When we pass an object, it will change outside the function.

## First Class Functions

### Passing Functions as Arguments

* Function facts
    * A function is an instance of the Object Type
    * A function behaves like any other object.
    * We can store functions in a variable
    * We can pass a function as an argument to another function
    * We can return a function from a function

```javascript

var years = [1990, 1985, 2003, 1993, 1979];

function arrayCalc(arr, fn){
    var arrRes = [];
    for (var i=0; i < arr.length; i++) {
        arrRes.push(fn(arr[i]));
    }
    return arrRes;
}

function calculateAge(el) {
    return 2020 - el;
}

function isFullAge(el) {
    returtn el >= 18;
}

var ages = arrayCalc(years, calculateAge);
console.log(ages);

var fullAges = arrayCalc(years, isFullAge);
console.log(fullAges);

```
* Callback Functions: Functions we write that will be called back later.
* This is better than having one function with two functions (or more) written inside.

### Functions Returning Functions

```javascript

function interviewQuestion(job) {
    if (job === 'designer') {
        return function(name) {
            console.log(name + ', can you please explain what UX design is?');
        }
    } else if (job === 'teacher') {
        return function(name) {
            console.log('What subject do you teach, ' + name + '?');
        }
    } else {
        return function(name) {
            console.log('Hello' + name);
        }
    }
}

var teacherQuestion = interviewQuestion('teacher');
teachQuestion('John'); // What subject do you teach, John?

interviewQuestion('teacher')('Mark'); // What subject do you teach, Mark?

```

* We can create one generic function and then create a lot more specific functions based off the initial function.
* Functions are always first class functions in javascript, because they are objects.

### Immediately Invoked Function Expressions (IIFE)

```javascript

function game() {
    var score = Math.random() * 10;
    console.log(score >= 5);
}
game();

```

* We can do the previous code in a better way:

```javascript

(function (bonus) {
    var score = Math.random() * 10;
    console.log(score >= 5 - bonus);
})(5);

```

* If you wrote the function without a name, it would think this was a function declaration.
* We basically need to trick the parser to think this is an expression not a declaration.
    * In javascript, what is in parenthesis cannot be a declaration.
* We can only call an IIFE once.
* Usable when we want to create a new scope that is hidden from the outside scope.

## Closures

```javascript

function retirement(retirementAge) {
    var a = 'years left until retirement.';
    return function(yearOfBirth) {
        var age = 2016 - yearOfBirth;
        console.log((retirementAge - age) + a);
    }
}

var retirementUS = retirement(66);
retirementUS(1990); // 40 years until retirement

retirement(66)(1990); // 40 years until retirement

```
* Closures: An inner function always has access to the variables and parameters of its outer function, even after the outer function has returned.
    * The scope chain always stays in tact
    * Normally, we'd expect `return` to just return the function name. Which it does, but the function that is returned still has access to the outer functions variables.
    * We call the new returned function, but with closure, the execution context will close in on the outer function.
* We don't create closures manually, it is built in to javascript.

## Bind, Call and Apply Methods

* In a nutshell, they allow us to call the function and set the this variable manually.

```javascript

var john = {
    name: 'John',
    age: 26,
    job: 'teacher'
    presentation: function(style, timeOfDay) {
        if (style === 'formal') {
            console.log('Good ' + timeOfDay + ' , Ladies and gentlemen! I\'m ' + this.name + ', I\'m ' + this.job);
        } else if (style === 'friendly') {
            console.log('Hey, what\'s up? I\'m ' + this.name + ', I\'m ' + this.job + 'Have a good ' + timeOfDay);
        }
    }
}

john.presentation('formal', 'morning');

```

* Call method:
    * Set the `this` method in the first variable:
    * `john.presentation.call(emily, 'friendly', 'afternoon')`;
    * Method borrowing.
* Apply method:
    * Apply accepts the argument as an array.
    * `john.presentation.apply(emily, ['friendly', 'afternoon']);`
        * This wouldn't work in the above example as it doesn't expect to receive an array.
* Bind method:
    * Very similar to the call method.
    * Allows us to set the this method explicitly.
    * Doesn't immediately call the function but creates a copy of the function that we can store.
    * `var johnFriendly = john.presentation.bind(john, 'friendly')`
    * Then if you call `johnFriendly('morning');`
        * In this example, we'd always have a function for the 'friendly' preset variable.
    * Currying - A function based on another function with preset parameters.  
