---
title: MySQL Bootcamp 12 - One to Many
created: '2020-02-05T06:55:02.210Z'
modified: '2020-02-05T19:02:47.598Z'
---

# MySQL Bootcamp 12 - One to Many

## Real World Data is Messy

* Real world data is messy and iterrelated
  * A blog: User data, info about blog posts, keep track of comments, tags, ads, what users are clickin on.

## Types of Data Relationships

* One to One Relationship
  * E.g. A customer and customer details table, where customer is commonly used details, username, password, email and details is billing and shipping address, birthday etc.
  * One customer to one customer details.
* One to Many Relationship
  * E.g. The relationship between books and reviews. One book can have many reviews.
    * But those reviews belong to one book.
* Many to Many Relationship
  * Two entities: Books and authors. Books can have many authors, and authors can have many books.

## One to Many: The Basics

* Customers & Orders
  * One customer can have many orders, orders can only have one customer associated to them.
* To create an example:
  * Customers: customer_id, first_name, last_name, email
  * Orders: order_id, order_date, amount, customer_id
  * To create a link, we use customer_id on both. By making the customer_id the primary key, we can then reference it somewhere else.
    * The primary key has to be unique in order to reference the data.
* Foreign Key
  * Reference to other tables within our table.

## Working with Foreign Keys

* Convention for foreign keys is: tablename_foreignkeycolumname i.e id -> customers_id
* To create a foreign key:

```SQL

CREATE TABLE orders(
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_date DATE,
    amount DECIMAL(8,2),
    customer_id INT,
    FOREIGN KEY(customer_id) REFERENCES customers(id)
);

```

## Cross Join

* The whole point of 'Joins' is that it takes 2 tables and conjoin them in a couple of different ways.
* `SELECT * FROM customers, orders` 
  * However, this is like multiplying them. It prints out every customer with every order. In every possible combination.

## Inner Join

* Rather than print all this data, a lot which doesn't match. We could use:
  * `SELECT * FROM customers, orders WHERE customers.id = orders.customer_id;`
  * `customers.id` - table.column
  * We have joined them together using an Implicit Inner Join
    * We have joined them when they match.
  * An inner join takes records from A and B where the conditions match.
  * This is an implicit way to do this, but there are is an explicit way.

### Explicit Inner Join

* `SELECT * FROM customers JOIN orders ON customers.id = orders.customer_id;`
* New keywords: JOIN
* Take the left and right tables and put them together where the conditions are met.
* Does the order matter?
  * Yes on *, but when we pick our data with a SELECT it doesn't change.

## Left Join

* Left Join - Select everything from A, along with any matching records in B
  * For customers & orders, it would take everything from customers and also the orders where they match.
  * Syntax: `SELECT * FROM customers LEFT JOIN orders ON customers.id = orders.customer_id;`
* How to replace NULL with zero.
  * `IFNULL(SUM(amount), 0)` - Is the first argument null, change it to the second. If not, leave it as the first.

## Right Join

* Select everything from B, along with matching records in A.
  * Wherever the join condition is met.
* Right Join can be useful when checking data against another table.
* For example, a customer database and employee database where every customer has a single sales employee.
  * Left Join would show all customers but not all employees
  * Right join would show all employees regardless of who their customers are.
* Is there a big difference if we just change the order of what we are joining?
  * No, just flipping the order.

## On Delete Cascade

* This basically means when a customers data is deleted, delete the orders associated with it.
* Syntax:

```SQL

CREATE TABLE orders(
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_date DATE,
    amount DECIMAL(8,2),
    customer_id INT,
    FOREIGN KEY(customer_id) REFERENCES customers(id) ON DELETE CASCADE
);

```







