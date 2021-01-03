---
title: JDSD 7 - Typescript
created: '2020-04-27T07:21:29.645Z'
modified: '2020-04-27T16:48:51.119Z'
---

# JDSD 7 - Typescript

* Dynamic <-> Static
* Strong <-> Weak

## Dynamic vs Static Typing

* Because Javascript is dynamically typed we can do:
  * var a = 100;
  * We don't have to define what type of variable a is going to be.
    * You have to declare variables explicitly.
  * c++ would have to use int a; and then a = 100.
  * With javascript, typechecking is done on runtime.
* Pro's of statically stype language: 
  * We get documentation. I can see what kind of parameters it expects.
    * function sum(a: number, b: number) {return a + b;}
  * It helps with autocompletion in text editors
  * You are going to get less bugs in production.
    * If it fails in compile time, before you send it on to the browser, then BOOM.
* Cons
  * We made our code a little bit more harder to read.
    * It takes time to learn.
    * It's an extra layer of complexity.
  * Why not just write better unit tests.
  * Slower development process.

## Weakly vs Strongly Typed

* A lot of people confuse strong and static.
* Javascript is a dynamically typed language that is weakly typed.
* var a = "boooooyaa", a + 17 // returns "boooooyaa17"
  * This is known as type coercion.
* In a strongly typed language, you can't do this.
  * For example, in python this would reject it because its a number and a string.

## Static Typing in Javascript

* Typescript is Microsoft, Flow and ReasonML are from Facebook.
* Flow is able to add types to javascript, put it through a Babel compiler and spits out Javascript.
  * A static type checker.
* Typescript differs, because it has its own compiler.
  * It is a superset of javascript, it adds functionality on top of javascript.
  * The growth of typescript is larger than all of the others.
* Reason is a separate language on its own. But is similar in syntax to javascript.
  * Reason compiles itself and churns out javascript from the compiler.

## TypeScript Compiler

* Need node installed. To check `node -v`
* `sudo npm install -g typescript`
* `tsc`
* A typescript has the file extension `.ts`

```typescript

const sum = (a, b) => {
  return a + b;
}

```
* run `tsc typescript.ts` and it will create typescript.js
  * This will return an ES5 compliant function (using var)
* Typescript lets us use new features, in line with javascript and compile it down as necessary.
* To write a type:

```typescript

const sum = (a : number, b : number) => {
  return a + b
}

```
* Some editors will have a spellchecker type function for types where they can see if it doesn't work.
* It does still compile but you can see there are errors.
* To auto-compile code: `tsc --init`, this has created a config file for typescript
  * This file gives us a lot of options.
  * Then in watch mode use `tsc typescript.ts --watch`

## Types in Typescript

### Boolean

* `let isCool: boolean = true`

### Number

* `let age: number = 56;`

### String

* `let eyeColor: string = 'brown';`
* `let favouriteQuote: string = `I'm not old, I'm only ${age}`;`

### Arrays

* `let pets: string[] = ['cat', 'dog', 'parrot'];`
* `let pets2: Array<string> = ['cat', 'dog', 'parrot'];`

### Objects

* `let wizard: object = {a: 'John'}`
  * Make sure this is lowercase.

### null and undefined

* `let meh: undefined = undefined;`
* `let nooo: null = null;`

### Tuple

* `let basket: [string, number];`
  * `basket = ['basketball', 5];`
* Needs a specific type and type order.
  * Couldn't do 5, basketball.

### Enum

* `enum Size { Small = 1, Medium = 2, Large = 3}`
  * `let sizeName: string = Size[2];`
  * You could also do: `let sizeName: number = Size.Small;`
* We can have a set of distinct sizes, directions etc.

### Any

* Be careful with this type.
* `let whatever: any = 'aghhhhhh';`
  * Whatever could then be whatever we wanted.
* Why would you want to use this?
  * Cases where you are converting javascript code to typescript code.
  * If things take time, this is kind of a stopgap.

### Void

* `let sing = (): void => {console.log('lalalalalalala')}`
  * This would work, because void is for a function that doesn't return anything.

### Never

* `let error = (): never => { throw Error('oooops'); }
* Never tests two things: 
  * That it never returns
  * A variable is never true.
* A function returning never cannot have a reachable endpoint.

### Interface

```typescript

interface RobotArmy { 
  count: number, 
  type: string, 
  magic: string 
}

//could also use type RobotArmy = (But type doesn't create a new name)

let fightRobotArmy = (robots: RobotArmy) => {
  console.log('FIGHT!')
}
// same as
let fightRobotArmy2 = (robots: {count: number, type: string, magic: string}) => {
  console.log('FIGHT!')
}

```
* Interface lets us use a custom type.
* Standard with interface to have capital start of names.
* More on type and interface: https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c

### Function

```typescript
let fightRobotArmy3 = (robots: RobotArmy): void => {
  console.log('FIGHT!')
}

let fightRobotArmy 4 = (robots: {count: number, type: string, magic: string}): number => {
  console.log('FIGHT!');
  return 5
}
```

### Class

```typescript

class Animal {
  private sing: string = 'lalala'
  constructor(sound: string) {
    this.sing = sound;
  }
  greet() {
    return `Hello ${this.sing}`
  }
}

let lion = new Animal('ROAR')
lion.greet();
lion.sing

```
* private creates a private variable that can't be accessed outside the class.


### Union

* `let confused: string | number = "hello"`
* Can chain different types together.


## Type Assertions

* This allows you to overwrite a type in any way you want.

```javascript

interface CatArmy {
  count: number,
  type: string,
  magic?: string
}

let dog = {} as CatArmy
dog.count

```
* The question mark means it may or may not be part of the object.
  * It means you don't need the magic property every time.
* Typescript will infer types if they are not explicitly designed.

## 3rd Party Libraries

* DefinitelyTyped
* 3rd Party libraries with Typescript.
* You can add files, which define libraries for typescript to understand.
  * `definitelytyped.org`
  * We can download files 
