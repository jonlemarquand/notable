---
title: MySQL Bootcamp 8 - Refining Our Selections
created: '2020-02-03T07:45:20.176Z'
modified: '2020-02-03T08:25:42.239Z'
---

# MySQL Bootcamp 8 - Refining Our Selections

## Using DISTINCT

* Something we use in connection with SELECT.
* Distinct will only give us the unique or distinct titles.
* `SELECT DISTINCT author_lname FROM books;`
  * If you wanted to know only the distinct author last names then you would use this to remove duplicates.
* What about distinct full names?
  * Could use CONCAT: `SELECT CONCAT(author_fname, ' ', author_lname) FROM books;`
  * More efficient: `SELECT DISTINCT author_fname, author_lname FROM books;`

## Sorting data with ORDER BY

* `SELECT author_lname FROM books ORDER BY author_lname;`
* Remember there is a space between ORDER and BY
* Sorted by default in alphanumeric ascending order.
  * To change: `SELECT author_lname FROM books ORDER BY author_lname DESC;` - Will now be in descending order.
* `SELECT title, author_fname, author_lname FROM books ORDER BY 2;`
  * This selects the 2nd thing written to sort by.
  * In this case author_fname.
* `SELECT author_fname, author_lname FROM books ORDER BY author_lname, author_fname;`
  * You can sort by multiple columns if you need.

## LIMIT

* Limit, limits the number of results given.
* `SELECT title FROM books LIMIT 3;`
* Example: I want the 5 most recently released books:
  * `SELECT title, released_year FROM books ORDER BY released_year DESC LIMIT 5;`
* `SELECT title, released_year FROM books ORDER BY released_year DESC LIMIT 0,5;`
  * 0 is the starting point, then 5 is the number of books selected.
  * This could be used for blog posts and pagination. (The first 10, the second 10 etc.)

## Better searches with LIKE

* "There's a book I'm looking for, can't remember the name. The author's first name is David, or Dan or Dave..."
  * You could search for "Da"
* `SELECT title, author_fname, author_Lname, WHERE author_fname LIKE '%da%';`
  * The % are just wildcards
* `... WHERE stock_quantity LIKE '____';`
  * An underscore is a wildcard character that specifies exactly one character
  * So the above example has 4 underscores.
  * Phone number search: `(235)234-0987 WHERE phone_number LIKE '(___)___-____';`
* What if I'm searching for something with a % sign in it or an _?
  * Use an escape character `WHERE title LIKE '%\%%`

