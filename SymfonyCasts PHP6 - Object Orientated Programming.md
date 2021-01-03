---
title: SymfonyCasts PHP6 - Object Orientated Programming
created: '2020-07-20T05:34:34.579Z'
modified: '2020-07-20T08:23:02.573Z'
---

# SymfonyCasts PHP6 - Object Orientated Programming

## A Class and an Object

```php

<?php

class Ship
{
  public $name;
  public $weaponPower = 0;
}

$myShip = new Ship();
$myShip->name = 'Jedi Starship';

echo 'Ship name:'.$myShip->name;
```
* We create objects from classes.
  * new tells PHP we're creating an object from a class.
* A class is like an empty template, it's not a ship in it's own right, but has all the fields to record information about it.
* An object is like the completed worksheet we've filled out for a ship.
  * We call data properties rather than keys.
  * With arrays, you can just invent a new key but with objects you need to pre-register the property in the class.
  * An object has a class that defines all possible properties it can hold.
    * A class is documented, where as an array would have no idea.
* The class gives us a skeleton and some rules we have to follow.
* You can also set a default value, so that if nothing is defined in the object, it will set to that.

### Class Methods

* Method is a fancy word for function.
* A method lives inside a class.

```php

class Ship
{

  public $name;
  public function sayHello()
  {
    echo 'HELLO!';
  }
  public function getName()
  {
    return $this->name;
  }
}

$myShip->sayHello();
echo $myShip->getName();
```

* This is what we use to access a variable inside the class.

#### Methods that Do Work

```php

public function getNameAndSpecs($useShortFormat)
{
    if ($useShortFormat) {
        return sprintf(
            '%s: %s/%s/%s',
            $this->name,
            $this->weaponPower,
            $this->jediFactor,
            $this->strength
        );
    } else {
        return sprintf(
            '%s: w:%s, j:%s, s:%s',
            $this->name,
            $this->weaponPower,
            $this->jediFactor,
            $this->strength
        );
    }
}

echo $myShip->getNameAndSpecs(false);
echo $myShip->getNameAndSpecs(true);

```
* `sprintf` and the `%s` act as a string formatter.

### Objects Interact

```php
class Ship
{
  public function doesGivenShipHaveMoreStrength($givenShip) {
    return $givenShip->strength > $this->strength;
  }
}


if ($myShip->doesGivenShipHaveMoreStrength($otherShip)) {
    echo $otherShip->name.' has more strength';
} else {
    echo $myShip->name.' has more strength';
}

```

## Using Objects

* We normally put classes in their own files.
  * Put class Ship in Ship.php just makes sense.
* `require_once __DIR__.'/lib/Ship.php;`
  * DIR is a constant that always points to the current directory.

## Private Access

* As soon as you make a property private, it can't be accessed from outside the class.
* Public functions:

```php
class Ship
{
    public function setStrength($number)
    {
        $this->strength = $number;
    }
    public function getStrength()
    {
      return $this->strength;
    }
}
```
* This enables you to, rather than accessing the stength property directly, access the setStrength property.
  * This also allows you to check the value being entered is right.
  * The idea of setting property to private and then using getters and setters is very common.

## Type Hinting

* Type hinting allows us to get better errors.
* It can be classes, strings, variables etc.
* `function battle(Ship $ship1);`

## The Constructor

* Whenever you create a project, you can hook into a project to call a function and set stuff up.

```php

public function __construct() {
  $this->underRepair = mt_rand(1, 100) < 30;
  $this->name = $name
}

public function isFunctional() {
  return !$this->underRepair;
}

```
* You can set up arguments to the constructor that forces the user to pass a name for creating an object.
