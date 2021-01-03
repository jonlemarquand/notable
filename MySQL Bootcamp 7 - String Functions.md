---
title: MySQL Bootcamp 7 - String Functions
created: '2020-01-30T20:05:11.395Z'
modified: '2020-02-02T10:21:13.045Z'
---

# MySQL Bootcamp 7 - String Functions

* You can create SQL code in a file and save it as a .sql file.
* To run it use `source file_name.sql;`
  * You have to refer to the file by the full path i.e. `source testing/insert.sql`

## Working with CONCAT

* How to combine data for cleaner output
* `CONCAT(column, anotherColumn);`
  * This will put them together but with no space in between.
* `SELECT CONCAT(author_fname, ' ', author_lname) AS 'full name' FROM books;`
  * This selects from the table books, and renames them as 'full name'.
* `SELECT author_fname AS first, author_lname AS last, CONCAT(author_fname, ' ', author_lname) AS full FROM books;`
  * This will print First Name, Last name and Combined Full name.

### CONCAT_WS

* To remove the repetitive filler ('-') you can use `CONCAT_WS(' - ', title, author_fname, author_lname) FROM books;`
  * This will put the ' - ' between each of the values separated by a comma.
  * Useful when combining a lot of data in one form.

## SUBSTRING

* Work with part of strings.
* Allows us to select individual portions of strings.
* The syntax: `SELECT SUBSTRING('Hello World', 1, 4);`
  * The string that we want to use ('Hello World')
  * The characters we want.
  * SQL starts at 1 rather than 0 (unlike javascript).
  * This will select characters one to four or 'Hell'
* If you only pass in one number it starts from whatever that index is and goes to the end.
  * i.e. `SELECT SUBSTRING('Hello World', 7);` will produce 'World'.
* If you use a negative number `SELECT SUBSTRING('Hello World', -3);` it will give you 'rld'.
  * It starts from the end of the string and works backwards.

### Working with Columns

* `SELECT SUBSTRING(title, 1, 10) AS 'short title' FROM books;`
  * This will select the whole column and rename it as well.
* You can also combine SUBSTRING with CONCAT as follows:

```SQL

SELECT
  CONCAT
  (
    SUBSTRING(title, 1,10),
    '...'
  ) AS 'short title'
FROM books;

```

## Introducing REPLACE

* Replaces parts of a string.
* Get rid of a certain occurance of a character, replace spaces with commas etc.
* `SELECT REPLACE('Hello World', 'Hell', '#$#@');
  * This will replace 'Hell' (2nd postion) with something else (3rd Position)
  * It will find all instances of this occurance and replace it.
  * It only replaces exactly those characters. It is case sensitive.
* `SELECT REPLACE(title, 'e ', '3') FROM books;`
  * This will replace all e's in the title of the table books.

## Using REVERSE

* Reverse reverses strings.
  * i.e. `SELECT REVERSE( 'Hellow World');` and we get dlroW olleH

## Working with CHAR_LENGTH

* It tells you hwo many characters are in a given string.
* `SELECT CHAR_LENGTH('Hello World');`
* `SELECT author_lname, CHAR_LENGTH(author_lname) AS 'length' FROM books;`

## Changing Case with UPPER and LOWER

* We can make something all caps or all lowercase letters.
  * `SELECT UPPER('Hello World');` - HELLO WORLD

