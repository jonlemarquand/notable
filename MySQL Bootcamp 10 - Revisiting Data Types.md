---
title: MySQL Bootcamp 10 - Revisiting Data Types
created: '2020-02-03T19:46:30.848Z'
modified: '2020-02-03T20:44:23.194Z'
---

# MySQL Bootcamp 10 - Revisiting Data Types

## CHAR and VARCHAR

* We've already been using `VARCHAR` to store text data.
* `CHAR` has a fixed length.
  * Every title would be '5 characters' in an example.
    * 10 Characters would leave characters out.
    * Less than 5 characters would pad it out with spaces.
* The longest CHAR is 255 characters.
* `CHAR` is faster for fixed length text.
  * State Abbreviations: NY, CA etc.
  * Yes/No Flags: Y/N
  * Sex: M/F
* The difference between storage space on `CHAR` and `VARCHAR` is neglible unless you are on a huge, huge application.

## DECIMAL

* `INT` is used for whole numbers.
* `DECIMAL(13,2)` - A way of creating a column that will store numbers after the decimal point.
  * The first number is the maximum number of digits (either side of the decimal)
    * Up to 65.
  * The second number is how many number of digits after the decimal point.
    * Between 0 and 30.
* If you go above the number, it will go to the largest possible decimal based on the constraints set up.
* If you specify 2 decimal places and write more, this will also round up to 2 decimal places.

## FLOAT and DOUBLE

* The differences between DECIMAL, FLOAT and DOUBLE are largely technical. It boils down to how they are stored.
  * The `DECIMAL` data type is a fixed-point type and calculations are exact.
  * The `FLOAT` and `DOUBLE` data types are floating-point types and calculations are approximate.
* FLOAT/DOUBLE store gigantic numbers and use less space. But it comes at the cost of precision.
  * FLOAT has precision issues after 7 digits. DOUBLE has precision issues after 15 digits.
* Always try and use decimal unless you know that precision doesn't matter.
  * Float only stores the first 7 digits.

## DATE, TIME and DATETIME

* `DATE` - Values with a date but not time.
  * 'YYYY-MM-DD' Format.
* `TIME` - Values with a time but no date.
  * 'HH:MM:SS' Format
* `DATETIME` - Values with a date AND time.
  * When something was updated, or when a post was commented on etc.
  * Format: 'YYYY-MM-DD HH:MM:SS'

## CURDATE(), CURTIME(), NOW()

* CURDATE() - gives current date
* CURTIME() - gives current time
* NOW() - gives current datetime

* For example: `INSERT INTO people (birthdate, birthtime, birthdt) VALUES ('Microwave', CURDATE(), CURTIME(), NOW());`
  * This is useful when adding a new user (for example) with the time when they register.

## Formatting Dates

* `SELECT name, DAY(birthdate) FROM people;` would extract the day from the date.
  * `DAYNAME(birthdate)` would pick the day.
  * `DAYOFYEAR(birthdate)` would show the day of the year that it is.
* These can also be used on DATETIME as well as date.
* There are also functions available with just TIME and DATETIME as well.
* `DATE_FORMAT` allows us to format the date in a particular way.
  * `SELECT DATE_FORMAT('2009-10-04 22:23:00', '%W %M %Y');` would give 'Sunday October 2009'
  * `SELECT DATE_FORMAT(birthdt, '%d/%m/%Y') FROM PEOPLE;` - Would give 02/11/2019 for example.

## Date Maths

* For example how do we say 'You commented on this to 2 Days 3 Hours ago'.
* `DATEDIFF(expr1, expr2)` - This will substract them and give you the result in days.
  * `SELECT DATEDIFF(NOW(), birthdate) FROM people;`
* `DATE_ADD(date, INTERVAL 1 SECOND)`
  * This will add one second to the specified date.
  * `SELECT birthdt, DATE_ADD(birthdt, INTERVAL 1 MONTH) FROM people;` - This will add a month on to the date.
* `date + INTERVAL expr`
  * `SELECT birthdt + INTERVAL 1 MONTH FROM people;`
  * You can also add `+ INTERVAL 10 HOUR + INTERVAL 15 MONTH` and chain them together.

## Working with TIMESTAMPS

* Storing metadata about when something is updated or changed.
* TIMESTAMPs are actually their own data type.
* DATETIME works for a bigger range '1000-01-01' to '9999-12-31' but TIMESTAMP only works between 1970 and 2038.
* TIMESTAMP takes up less space.
* `CREATE TABLE comments (content VARCHAR(100), created_at TIMESTAMP DEFAULT NOW());`
  * This will show a created at timestamp.
* `...changed_at TIMESTAMP DEFAULT NOW() ON UPDATE CURRENT_TIMESTAMP);`
  * If content is changed, it will update with the current timestamp.
  * CURRENT_TIMESTAMP or NOW() do the same thing.


