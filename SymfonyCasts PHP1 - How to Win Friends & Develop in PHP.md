---
title: SymfonyCasts PHP1 - How to Win Friends & Develop in PHP
created: '2020-07-16T20:16:52.115Z'
modified: '2020-07-19T13:00:30.777Z'
---

# SymfonyCasts PHP1 - How to Win Friends & Develop in PHP

## Let's Write some PHP!

* To write PHP:

```php

<div class="container">
  <h1><?php echo 'Look mam, PHP!'; ?></h1>
</div>

```
* This writes code in PHP.
* Unless we use print, it won't appear in the HTML source code.
* For variables:

```php

<div class="container">
  
  <?php 
    $cleverWelcomeMessage = 'All the love, none of the crap!';
  ?>
  
  <h1><?php echo $cleverWelcomeMessage; ?></h1>
</div>

```

## Functions

* PHP also has functions - Like javascript `rand();`
* PHP often uses the word `parameter` in place of `argument` in its documentation and error messages. These two words mean the same thing.
* Functions are machines that do work and return a value, arguments are variables that less us control the function.
* Functions work from the inside out.
  * If you have `<?php echo strtoupper(strtolower($cleverWelcomeMessage)); ?>` it will return in upper case because that executes last.

## Arrays and Loops

* An array can be set by either `$pets = array();` or `$pets = [];`
* To loop over an array in PHP:

```php

<?php

  foreach ($pets as $pet) {
    echo '<div class="col-lg-4">';
    echo '<h2>';
    echo $pet;
    echo '</h2>';
    echo '</div>';
  }

?>

```
* `foreach` is a language construct, it looks and acts like a function but has some special attributes.
  * Language constructs don't need semi-colons.
* `var_dump` is a way to print a variable to a screen and gives more info about it.
  * Arrays can have multiple different types.
  * Call it with `$pets[3]`;

### Associative Arrays

```php

<?php 

  $pancake = array(
    'name' => 'Pancake',
    'age' => '3 years',
  );

?>

```

* This is known as an associative array. Arrays with auto-assigned keys are known as index arrays.
* To call it: `<?php echo $pancake['name'] ?>`

### Arrays in Arrays

* Every array key has to be a string or an integer.
  * However, the value can be any data type.
* `die();` - temporarily stops the rest of the script from running.
  * Useful for debugging.

```php
<?php 

  $pet1 = array(
    'name' => 'Ralph',
    'age' => 3
  );

  $pancake = array(
    'name' => 'Pancake',
    'age' => '3 years',
  );

  $pets = array($pet1, $pancake);
  var_dump($pets[1]['age']);die();

?>

```
* Rather than echoing HTML code, you can close the php on each line which allows you to just write HTML.
* PHP reads a file from top to bottom like a book, so you need to define a variable before reading it.

## Working with Files, JSON and Booleans

* PHP can read JSON text format.
  * `$petsJson = file_get_contents('pets.json');`
* PHP Datatypes:
  * Strings
  * Numbers: Integers and Floats (3.1415)
  * Arrays
  * Booleans (TRUE or FALSE)
* To turn JSON into a PHP array:
  * `$pets = json_decode($petsJson, true);`

## IF Statements

```php

<?php 

if(array_key_exists('age', $cutePet)) {
  echo $cutePet['age'];
} else {
  echo 'Unknown';
}

if (!array_key_exists('age', $cutePet) || $cutePet['age'] == '') {
  echo 'Unknown';
} elseif ($cutePet['age'] == 'hidden') {
  echo '(Contact owner for age)';
} else {
  echo $cutePet['age'];
}

?>

```

## Creating Functions

```php

<?php 

function get_pets() {
  $petsJson = file_get_contents('data/pets.json');
  $pets = json_decode($petsJson, true);

  return $pets;
}

?>

```

### Using Require to Include Functions

* To load functions from one page to another, you can use require.

```php

<?php

  require 'functions.php'

?>

```

* Put the functions file in a lib folder, since it isn't meant to be accessed directly.
* If the last thing you have in a file is PHP code, you don't need to close the PHP tag
* Require, require once
  * Require will always load the file, require once will only load once.
  * Also include, include once.
    * If the file doesn't exist, include lets the script keep running.
    * If the file doesn't exist with require, the page will stop working entirely.

## Adding a very simple layout

* Import header and footer into their own files.
* require the header at the top of each page, and the footer at the bottom after specific code:
  * `<?php require 'layout/header.php>'; ?>`
