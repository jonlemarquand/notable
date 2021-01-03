---
title: MySQL Bootcamp 9 - The Magic of Aggregate Functions
created: '2020-02-03T09:07:50.639Z'
modified: '2020-02-03T10:07:39.517Z'
---

# MySQL Bootcamp 9 - The Magic of Aggregate Functions

* Focusing on 'Reading' our data (In CRUD terms)
* Aggregate or combine data to get meaning out of it.

## The COUNT function

* How many books are in our database?
  * `SELECT COUNT(*) FROM books;`
* How many author first names are in our database?
  * `SELECT COUNT(DISTINCT author_lname, author_fname) FROM books;`
* How many titles contain "the"?
  * `SELECT COUNT(*) FROM books WHERE title LIKE '%the%';`

## The Joys of Group By

* "GROUP BY summarises or aggregates identical data into single rows".
* `SELECT title, author_lname FROM books GROUP BY author_lname;`
  * On a visual level, this looks like it is just giving combined authors, and then just giving us the first book.
  * What's actually happening is that it is just grouping based on last name, and treating it as one row. Behind the scenes they are grouped together.
* `SELECT author_lname, COUNT(*) FROM books GROUP BY author_lname;`
  * It's counting how many sub rows are in these things.

## MIN and MAX

* Find the minimum released year in the table:
  * `SELECT MIN(released_year) FROM books;`
* Find the longest book:
  * `SELECT MAX(pages) FROM books;`
* What if I want the title of the longest book?
  * `SELECT * FROM books WHERE pages = (SELECT Min(pages) FROM books);`
    * This is a subquery, that we'll come back to later.
  * Or `SELECT * FROM books ORDER BY pages DESC LIMIT 1;`

## MIN and MAX with Group By

* Find the year each author published their first book:
  * `SELECT author_fname, author_lname, Min(released_year) FROM books GROUP BY author_lname, author_fname;`

## The SUM function

* It adds things together.
* `SELECT Sum(pages) FROM books;`
* Sum all pages each author has written:
  * `SELECT author_fname, author_lname, Sum(pages) FROM books GROUP BY author_lname, author_fname;`

## The AVG Function

* It will sum things together and then divide based on how many things have been added.
* Calculate the average release year across all books:
  * `SELECT AVG(release_year) FROM books;`
* Average gives you 4 decimal points.
* Calculate the average stock quantity for books released in the same year:
  * `SELECT released_year, AVG(stock_quantity) FROM books GROUP BY released_year;`


