---
title: MySQL Bootcamp 18 - Database Triggers
created: '2020-02-09T16:08:52.137Z'
modified: '2020-02-10T07:48:58.673Z'
---

# MySQL Bootcamp 18 - Database Triggers

* SQL statements that are automatically run when a specific table is changed.
* The syntax:

```SQL

CREATE TRIGGER trigger_name
  trigger_time trigger_event ON table_name FOR EACH ROW
  BEGIN
  ...
  END;

```

* Trigger time: Before or After
  * Do you want the code to run before/after the event happens
* trigger_event: insert, update, delete
  * Run something before it is inserted or deleted
* Two potential uses:
  * Check and validate to see if an age is over 18 (although this should be done in the app rather than the database)
  * Add something to an 'Unfollowed' table when a record is deleted (because someone has stopped following someone)

## Writing our first Trigger: A Simple Validation

```SQL

DELIMITER $$

CREATE TRIGGER must_be_adult
  BEFORE INSERT ON people FOR EACH ROW
  BEGIN
    IF NEW.age < 18
    THEN
      SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXXT = 'Must be an adult!';
    END IF;
  END;
$$

DELIMITER;

```

* `must_be_adult` is just a name for the trigger.
* what ever is in between before and end is the SQL code that will run.
  * NEW.age - just refers to the 'new' data that will be inserted in the table.
* `DELIMITER $$`
  * A semi-colon is basically a delimiter. It would end the line and run the code.
  * To stop this happening, we use DELIMITER $$ which sets the delimiter to two dollar signs.
  * Then use $$ at the end to run the code and set DELIMITER ; back at the end.

### MySQL Errors

* A numeric error code (1146). This number is MySQL-specific
* A five-character SQLSTATE value ('42S02').
  * The values are taken from ANSI SQL and ODBC and are more standardised.
* A message string- textual description of the error.
* If we get an error `this number` it's easier to check against and use in code.
* 45000 is a generic state representing "unhandled user-defined exception".

## Creating Logger Triggers

* We want to make a record of when someone unfollows someone.

```SQL

DELIMITER $$

CREATE TRIGGER create_unfollow
    AFTER DELETE ON follows FOR EACH ROW 
BEGIN
    INSERT INTO unfollows
    SET follower_id = OLD.follower_id,
        followee_id = OLD.followee_id;
END$$

DELIMITER ;

```

## Managing Triggers

* Listing Triggers: `SHOW TRIGGERS;`
* Removing Triggers: `DROP TRIGGER trigger_name;`
* A word of warning: Triggers can make debugging hard!
  * Triggers are stealh bugs waiting to happen. It happens behind the scenes in the database.
  * Some people chain triggers together.
    * This could be useful but is frowned upon.
  * Always aim to avoid triggers as much as possible.
