---
title: You Don't Know JS 2.3 (Scopes & Closures) - Function vs Block Scope
created: '2020-01-10T16:49:36.370Z'
modified: '2020-01-10T16:52:58.588Z'
---

# You Don't Know JS 2.3 (Scopes &amp; Closures) - Function vs Block Scope

## Scope From Functions

Let's explore function scope and its implications:

```javascript

function foo(a) {
    var b = 2;

    // some code

    function bar() {
        // ...
    }

    // more code

    var c = 3;

}
```
In this snippet, the scope bubble for `foo(..)` includes identifiers a, b, c and bar. It doesn't matter where in the scope a declaration appears, the variable or function belongs to the containing scope bubble, regardless.
