---
title: You Don't Know JS 2.1 (Scopes & Closures) - What is Scope?
created: '2019-12-11T17:29:42.426Z'
modified: '2020-01-10T16:37:10.562Z'
---

# You Don't Know JS 2.1 (Scopes &amp; Closures) - What is Scope?

## Compiler Theory

* JavaScript is a compiled language, althoug it isn't compiled well in advance.
* In a traditional compiled-language process, a chunk of source code, your program, will undergo three steps before it's executed:
  * Tokenising/Lexing
    * Breaking up a string of characters into meaningful chunks, called tokens.
    * For example, `var a = 2;` would be broken up into `var`, `a`, `=`, `2`, and `;`.
  * Parsing
    * Taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program.
    * This tree is called an "AST" (Abstract Syntax Tree)
  * Code-Generation
    * The process of taking an AST and turning it into exectuable code.
* Javascript engines don't have the luxury of compiling this code in a build step ahead of time.
  * For JavaScript, the compilation that occurs happens, in many cases, mere microseconds (or less!) before the code is executed.

## Understanding Scope

### The Cast

* Engine: Responsible for start-to-finish compilation and execution of our JavaScript program.
* Compiler: Handles all the dirty work of parsing and code-generation.
* Scope: Collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

### Back &amp; Forth

* How will <em>Engine and Friends</em> approach the program `var a = 2;`?
  * The first thing the Compiler will do is break it down into tokens, which it will then parse into a tree.
  * Compiler will then proceed as:
    * Encountering `var a`, Compiler asks Scope to see if a variable `a` already exists for that particular scope collection. 
      * If so, Compiler ignores this declaration and moves on.
      * Otherwise, Compiler asks Scope to declare a new variable called `a` for that scope collection.
    * Compiler then produces code for Engine to later execute, to handle the `a = 2` assignment.
      * The code Engine runs will first ask Scope if there is a variable called `a` accessible in the current scope collection. If so, Engine uses that variable. If not, Engine looks elsewhere.
  * If Engine finds a variable, it assigns the value 2 to it. If not, Engine will raise its hand and yell out an error!

  ### Nested Scope

  * If a variable cannot be found in the immediate scope, Engine consults the next outer containing scope, continuing until found or the global scope has been reached.
  * If a RHS look-up fails to ever find a variable anywhere in the nested scopes, this will result in a `ReferenceError`.
  * If a LHS look-up arrives at a global scope without finding it (and the program is not running in "strict mode") then the gloabl scope will create a new variable of that name in the global scope, and hand it back to Engine.
