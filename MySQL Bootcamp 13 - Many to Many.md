---
title: MySQL Bootcamp 13 - Many to Many
created: '2020-02-06T07:02:57.435Z'
modified: '2020-02-06T07:45:56.623Z'
---

# MySQL Bootcamp 13 - Many to Many

* Books and authors - books can have many authors, authors can have many books.
* Imagine we're building a TV show reviewing application.
  * Associating reviewers with tv shows.
  * They are connected through a third table, Series Data <----Reviews Data---->Reviewers
    * This is known as a join or union table.

## Creating Tables

```SQL

SELECT title, rating, CONCAT(first_name, ' ', last_name) AS reviewer
FROM reviewers
INNER JOIN reviews
	On reviewers.id = reviews.reviewer_id
INNER JOIN series
	On series.id = reviews.series_id;

```

* Just to INNER JOIN statements together.
