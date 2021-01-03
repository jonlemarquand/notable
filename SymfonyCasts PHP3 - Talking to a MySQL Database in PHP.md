---
title: SymfonyCasts PHP3 - Talking to a MySQL Database in PHP
created: '2020-07-18T15:10:20.022Z'
modified: '2020-07-19T13:00:44.541Z'
---

# SymfonyCasts PHP3 - Talking to a MySQL Database in PHP

## How to Speak Database

* Queries are kind of human sentences that describe the data you want.
* MySQL is not the database but just talks to the database.
* `mysql -h localhost --port==3306 -u root -p`
  * Try root or blank for password.

## Talking to Databases in PHP

* `$pdo = new PDO('mysql:dbname=air_pup;host=localhost', 'root', null);`
  * This is how to connect to the database in PHP.
```php

$result = $pdo->query('SELECT * FROM pet');
// Produce an array for each row in the table.
$rows = $result->fetchAll();

```

## Object-Oriented Intro: Classes and Objects

* To talk to the database, we opened a connection using a class.
  * A function that has an argument like anything else.
  * Instead of returning a string, it returns a PDO object.
* Objects are a data type like strings, booleans.
  * Class and object are similar...ish
* Some functions aren't global, they live in objects. We call a function through it's object: `pdo->query('SELECT * FROM pet');`
* Each object has a few set of functions they can call.
* There are a lot of deprecated PHP functions.

## Central Configuration

* To make it easier to control the app, it's easier to add central functions to their own file.
* `config.php` can contain the database name, username and password.
  * This file isn't meant to be a page, it's going to be required from other pages.

```php

<? php

return array(
  'database_dsn' => 'mysql:dbname=air_pup;host=localhost',
  'database_user' => 'root',
  'database_pass' => null,
);

//in a file:

$config = require 'config.php';

$pdo = new PDO(
  $config['database_dsn'],
  $config['database_user'],
  $config['database_pass']
);
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

```

## Limiting the Number of Results

* The query becomes `SELECT * FROM pet LIMIT 3`

### Optional Function Arguments

```php

<?php

  $query = 'SELECT * FROM pet';
  if($limit != 0) {
    $query = $query.' LIMIT '.$limit;
  }

?>

```

## Query Parameters

* The HTTP request are accessed by the _GET superglobal.

```php

// In the new show.php for one pet

<?php 
  require 'lib/functions.php';
  $id = $_GET['id'];
  $pet = get_pet($id);
?>
<?php require 'layout/header.php'; ?>
<h1>Meet <?php echo $pet['name']; ?></h1>

<div class="container">
    <div class="row">
        <div class="col-xs-3 pet-list-item">
            <img src="/images/<?php echo $pet['image'] ?>" class="pull-left img-rounded" />
        </div>
        <div class="col-xs-6">
            <p>
                <?php echo $pet['bio']; ?>
            </p>

            <table class="table">
                <tbody>
                    <tr>
                        <th>Breed</th>
                        <td><?php echo $pet['breed']; ?></td>
                    </tr>
                    <tr>
                        <th>Age</th>
                        <td><?php echo $pet['age']; ?></td>
                    </tr>
                    <tr>
                        <th>Weight</th>
                        <td><?php echo $pet['weight']; ?></td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</div>
<?php require 'layout/footer.php'; ?>

```

### Query for One Pet

```php

function get_pet($id) {
  $query = 'SELECT * FROM pet WHERE id = '.$id;
  $result = $pdo->query($query);

  return $result->fetch();
}

```

* Use fetchAll: Multiple rows from database
  * Use fetch: One row from database.

## Preventing SQL Injection Attacks with Prepared Statements

* Change query functions:

```php

$query = 'SELECT * FROM pet WHERE id = :idVal';
$stmt = $pdo->prepare($query);
$stmt->bindParam('idVal', $id, PDO::PARAM_INT);
$stmt->execute();

return $stmt->fetch();


```
