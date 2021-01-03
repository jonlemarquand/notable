---
title: Laracasts PHP1 - PHP for Beginners
created: '2020-07-19T14:51:54.377Z'
modified: '2020-07-19T18:38:13.276Z'
---

# Laracasts PHP1 - PHP for Beginners

## Classes

* A class can represent any possible thing in your project.
  * Often we say, look for the nouns.
  * The representation or blueprint for some concept in your application.
  * Class is a representation for one thing. Therefore generally use the singular.

```php

<?php 


class Task {

  protected $description;

  protected $completed = false;

  public function __construct($description)

  {

    $this->description = $description;

  }

  public function complete() {
    $this->completed = true;
  }

  public function isComplete() {
    return $this->completed;
  }

}

$task = new Task('Go to the store');

$task->complete();

$task->isComplete();


?>

```

* An instance of a class is an object.
* `public` and `protected` are how we define visibility
  * if a variable is set to protected, it can't be echoed.
* The __construct method will be triggered any time you say new and then classname.
  * Then any argument you provide will be sent through to the constructor. You can then accept those and assign them.
  * You can then use any number of methods to do something with this data.

## PDO Refactoring

```php

// database/Connection.php

<?php

class Connection

{

  public static function make()

}


```

* Static methods shorten the code needed. Situations where you don't need an instance of a class:

```php

$connection = new Connection();
$connection->make();

Connection::make();

```

* Refactoring is restructuing the code but the underlying functionality is the same.
