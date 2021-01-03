---
title: You Don't Know JS 2.2 (Scopes & Closures) - Lexical Scope
created: '2020-01-10T16:37:34.795Z'
modified: '2020-01-10T16:46:20.457Z'
---

# You Don't Know JS 2.2 (Scopes &amp; Closures) - Lexical Scope

* There are two predominant models for how scope works: Lexical Scope (examined in this chapter) and Dynamic Scope.

## Lex-time

* The first traditional phase of a standard language compiler is called lexing.
  * The lexing process examines a string of source code characters and assigns semantic meaning to the tokens as a result of some stateful parsing.
* Lexical scope is scope that is defined at lexing time.
  * In other words, lexical scope is based on where variables and blocks of scope are authored, by you, at write time, and thus is (mostly) set in stone by the time the lexer processes your code.
* avoid using `eval` and `with`

