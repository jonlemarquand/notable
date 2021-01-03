---
title: MySQL Bootcamp 11 - The Power of Logical Operators
created: '2020-02-04T07:19:31.040Z'
modified: '2020-02-04T08:00:14.677Z'
---

# MySQL Bootcamp 11 - The Power of Logical Operators

## Not Equal

* Not Equal `!=`
  * `SELECT title FROM books WHERE year != 2017;`
  * Works by excluding data - the opposite of equals.

## Not Like

* Like allows us to match patterns in strings.
  * `SELECT title FROM books WHERE title LIKE 'W%';`
  * Any title with the letter 'w'
* Not like is excluding:
  * `SELECT title FROM books WHERE title NOT LIKE 'W%';`

## Greater Than

* For example `SELECT * FROM books WHERE released_year > 2000;`
  * This wouldn't include 2000.
  * `SELECT * FROM books WHERE released_year >= 2000;` - This would include 2000.
* `SELECT 99 > 1;` - This is a boolean. It has either 1 if it is true, or 0 if it is false.
  * String comparisons ('a' > 'b'): Generally avoid.
  * Uppercase is equivalent to lowercase in MySQL.

## Less Than

* Works the opposite way to Greater Than
* `SELECT * FROM books WHERE released_year <= 2000;`

## Logical AND

* `&&` - Allows us to chain multiplee things together where all statements are true.
* Either use:
  * `SELECT * FROM books WHERE author_lname='Eggers' AND released_year > 2010;`
  * `SELECT * FROM books WHERE author_lname='Eggers' && released_year > 2010;`

## Logical OR

* `||` or `OR`
* `SELECT title, author_lname, released_year FROM books WHERE author_lname = 'Eggers' || released_year > 2010;`
  * Or shows only one condition has to be true but both can.

## Between

* Allows us to select things between two range of values.
* `SELECT title, released_year FROM books WHERE released_year BETWEEN 2004 AND 20015;`
  * Between is inclusive.
* Not Between is also a thing.
  * `SELECT title, released_year FROM books WHERE released_year NOT BETWEEN 2004 AND 20015;`
* A note about comparing dates: It's best to use CAST() to change the string to the same format (DATETIME)
  * `SELECT CAST('2017-05-02' AS DATETIME);`

## In and Not In

* We could just use multiple `OR` but there's an easier way `IN`:
  * `SELECT title, author_lname FROM books WHERE author_lname IN ('Carver', 'Lahiri', 'Smith');`
* There's also a `NOT IN`:
  * `SELECT title, author_lname FROM books WHERE author_lname NOT IN ('Carver', 'Lahiri', 'Smith');`
* These can be combined with other operators such as:
  * `SELECT title, released_year FROM books WHERE released_year >= 2000 AND released_year NOT IN (2000,2002,2004,2006,2008,2010,2012,2014,2016);`

## Case Statements

* Has to do with logic but allows us to conditionally do something.

```SQL

SELECT title, released_year,
  CASE
    WHEN released_year >= 2000 THEN 'Modern Lit'
    ELSE '20th Century Lit'
  END AS GENRE
FROM books;

```
* Always use when ... then ... else
* However, when you use multiple cases then you use When something then, when something then, else
  * Else is the 'if nothing else is true' option.
  * No commas on when then statements.


