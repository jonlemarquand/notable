---
title: You Don't Know JS 2.5 (Scopes & Closures) - Scope Closure
created: '2020-01-28T20:03:09.882Z'
modified: '2020-02-09T18:18:23.817Z'
---

# You Don't Know JS 2.5 (Scopes &amp; Closures) - Scope Closure

* Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

```javascript

function foo() {
  var a = 2;

  function bar(){
    console.log(a); // 2
  }

  bar();

}

foo();

```

* The code above shows function `bar()` has access to the variable `a` in the outer enclosing scope because of lexical scope look-up rules.
  * However, this is only part of what closure is.
  * `bar()` has a closure over the scope of `foo()`. Why? Because `bar()` appears nested inside of `foo()`
* To put this in another way:

```javascript

function foo() {
  var a = 2;

  function bar() {
    console.log( a );
  }

  return bar;

}

var baz = foo();

baz(); // 2

```


