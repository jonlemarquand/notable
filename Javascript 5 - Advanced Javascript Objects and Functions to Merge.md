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
* This array calculator can be used with multiple functions input into it, as seen above.