---
title: 'SymfonyCasts PHP8 - OOP3: Inheritance, Abstract Classes, Interfaces and More'
created: '2020-07-25T13:22:13.093Z'
modified: '2020-07-26T08:38:36.953Z'
---

# SymfonyCasts PHP8 - OOP3: Inheritance, Abstract Classes, Interfaces and More

## Extends

* Create a new PHP class called `RebelShip.php`

```php

class RebelShip extends Ship
{

}


//index.php

$rebelShip = new RebelShip('My new rebel ship');
$ships[] = $rebelShip;

```
* When a class extends a parent class, it inherits all the original class data.
* Extending classes is great for reusing code without the duplication.

## Override

```php

if ($shipData['team'] == 'rebel') {
    $ship = new RebelShip($shipData['name']);
} else {
    $ship = new Ship($shipData['name']);
}

//Ship.php

public function getType()
{
  return 'Empire';
}

//RebelShip.php

    public function getType()
    {
        return 'Rebel';
    }

```
* In this example, RebelShip copies the entire blueprint of Ship but it can replace any of those pieces.
* A key part of this is that the parent `getType` class is never called for all rebel ship objects as it is completely replaced.

## Protected Visibility

* If a function or property is private, you can only access it from within the class.
  * Therefore only things inside of a 'Ship' class can access private ship functions. This does not extend to subclasses.
* Protected on the other hand, allows subclasses able to access protected functions.
* The more things you have marked as private, the easier it is to maintain a codebase later.
* The moral of the story is this, 
  * Make things private at first, 
  * Proctected once you need to access them in a subclass. 
  * Public when you need to use it outside of its class and subclass.

## Calling Parent Class Methods

```php

    public function getNameAndSpecs($useShortFormat = false)
    {
        $val = parent::getNameAndSpecs($useShortFormat);
        $val .= ' (Jedi)';
        return $val;
    }

```

## Creating an Abstract Ship

* Rather than duplicating a class and then not using stuff, create an abstract class (the most basic information that is used across both Rebel Ship and Ship)
* This is a new way of thinking about class heirarchy.
  * Each file would be simpler and only contain the code they need.

### Abstract Classes: Part 2



