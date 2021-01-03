---
title: MySQL Bootcamp 4 - Inserting Data
created: '2020-01-29T21:13:44.580Z'
modified: '2020-01-29T21:50:52.505Z'
---

# MySQL Bootcamp 4 - Inserting Data

## Insert

The syntax being used:

```SQL

INSERT INTO cats(name, age)
VALUES ('Jetson', 7);

```

* This is specifying the table, the column names and then the values that are being inserted into it.
* The order you write the column names matter.
  * If you say age first, you would have to write the age first.

### How do I see the data and know its there?

* To view all the data from any given table: `SELECT * FROM cats;`
* More in the next section.

### Multiple Insert

```SQL

INSERT INTO cats(name, age)
VALUES ('Charlie', 10)
      ,('Sadie', 3)
      ,('Lazy Bear', 1);

```

* Use a comma separated list.
  * The comma position doesn't matter but often you will see it at the start.

### Inserting a String that has Quotations

* You can do it a couple of ways
  * Escape the quotes with a backslash:
    * `"This text has \"quotes\" in it`
  * Alternate single and double quotes:
    * `"This text has 'quotes' in it"`

## A Note on Warnings

* If you see a warning when adding data use `SHOW WARNINGS`
  * When working in shell you have to use show warnings straight after.
* For example, if you go over the character limit, you will get a warning.

## NULL and NOT_NULL

* Null has no specified value.
  * Null does not mean zero.
* We can specify when we define a table that a field should be `NOT NULL`.
  * If you don't specify a value, you will then get a warning.

```SQL

CREATE TABLE cats2
  (
    name VARCHAR(100) NOT NULL,
    age INT NOT NULL
  );

```

## Setting Default Values

To set Default Values:

```SQL

CREATE TABLE cats3
  (
    name VARCHAR(100) DEFAULT 'unnamed',
    age INT DEFAULT 99
  );

```

* If you have default values, why do you need `NOT NULL`?
  * You would then be able to set values to NULL. Define it as explicitly empty value.

## A Primer on Primary Keys

* We want all of our data to be uniquely identifiable.
* If we have 5 dogs, all with the same age and name, how do you reference them?
* How we solve this is to get a unique identifier. An ID.
  * This is known as the Primary Key.
* How this works:

```SQL

CREATE TABLE unique_dogs (dog_id INT NOT NULL AUTO_INCREMENT
                        ,name VARCHAR(100)
                        ,age INT
                        ,PRIMARY KEY (dog_id));

```
* You get a new error if you get a Duplicate entry for Primary Key
* You can add the `AUTO_INCREMENT` so that the unique number automatically increases with every row that is inserted.

