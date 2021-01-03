---
title: Laracasts PHP2- Object-Oriented Principles in PHP
created: '2020-07-27T06:53:59.084Z'
modified: '2020-07-28T16:50:26.901Z'
---

# Laracasts PHP2- Object-Oriented Principles in PHP

## Classes

* Classes are like blueprints or templates. They define the general structure or behaviour of something in your codebase.
* For naming classes, find the noun: Project, Task, House, Comment
  * You can't hardcode specifics on the class when one project would be about this, another about that. Etc.

```php

class AchievementBadge
{
  public $title;
  public $description;
  public $points;

  public function awardTo($user)
  {
    //
  }

}

class Route
{
  //URI with a given class of endpoint
}

class Invoice
{
  public $id;
  public $duedate;

}

class InvoiceItem
{
  public $task;
}

```
* As we begin to create various instances of this class, each will have its own title, description

## Objects

* Instances of the class are known as objects.

```php

class Team
{
  protected $name;
  protected $members = [];
  
  public function __construct($name, $members)
  {
    $this->name = $name;
  }

  public static function start(... $params)
  {
    return new static($name);
  }

  public function name()
  {
    return $this->name;
  }

  public function members()
  {
    return $this->members;
  }

  public function add($name)
  {
    $this->members[] = $name;
  }

}

$acme = Team::start('Acme');
$acme->add('John Doe'); // Will add to the array with John Doe
```
* Using the `Team::start` will create a global variable, however, because this is just used to create a new team with language that people understands, it can be used.
* If we use `... $params` - This will destructure and return the entry values as an array.
* If a class is like a blueprint, and object is each implementation of the blueprint.

## Inheritance

```php

class CoffeeMaker
{
  public function brew()
  {
    var_dump('Brewing the coffee');
  }
}

// "is a"
class SpecialCoffeeMaker extends CoffeeMaker
{
  public function brewLatte()
  {
    var_dump('Brewing a latte');
  }
}
(new SpeicalCoffeeMaker())->brew();

```
* In this example, a special coffee maker is a coffee maker.
* In the case above, the child inherited the behaviour of the parent but can also extend its functionality.
* Default to a value, but let the child overwrite:

```php

class AchievementType
{
  public function name()
  {
    // Achievement Type
  }

  public function difficulty()
  {
    return 'intermediate';
  }
}

class FirstThousandPoints extends AchievementType
{
  public function qualifier($user)
  {

  }

  public function name()
  {
    return 'Welcome Aboard!';
  }
}

```

## Abstract Classes

* A lot of classes can find themselves repeating.
* With abstract class, they can never be instantiated (called), only the subclass can be called.

```php

abstract class AchievementType
{
  public function name()
  {
    $class = (new ReflectionClass($this))->getShortName();
    return trim(preg_replace('/[A-Z]]/', ' $0', $class));
  }

  abstract public function qualifier($user);
}

class ReachTop50 extends AchievementType
{
  public function qualifier($user)
  {

  }
}

$achievement = new ReachTop50();

echo $achievement->icon();

```
* The `abstract public function` lets PHP know that in the extending class, there should be something.
* The abstract keyword:
  * Removes the ability to instantiate the class.
  * It can also declare abstract methods - Methods which the class does not implement directly, but provides the basis and defering the specifics to the child class.

## Handshakes and Interfaces

```php

class CampaignMonitor
{

  public function __construct($apiKey)
  {
    $this->apiKey = $apiKey;
  }


  public function subscribe($email)
  {
    //
  }

}

class Drip
{

  public function subscribe($email)
  {
    //
  }

}

class NewsletterSubscriptionController
{

  public function store(CampaignMonitor $newsletter)
  {
    $newsletter->subscribe(auth()->user()->email);
  }

}

```
* Both campaign monitor and drip could have differernt subscribe methods, but the same result.
  * Often all you need is no formal agreement, just a handshake.
  * We are assuming some sort of automatic resolution.
* We could just use 'duck typing' - If it walks like a duck, and quack likes a duck, that's good enough.
  * This is not using type hinting.
* You could also use something more formal:
  * An interface - a class without behaviour. Only method signitures.
  * An interface doesn't care how you subscribe the user, it only cares that you expose the behaviour you can subscribe.
  * The previous handshake has become a formal contract with interface.

```php

interface Newsletter
{
  public function subscribe($email);
}

class Drip implements Newsletter
{
  public function subscribe($email)
  {
    //
  }
}

class NewsletterSubscriptionController
{

  public function store(Newsletter $newsletter)
  {
    $newsletter->subscribe(auth()->user()->email);
  }

}

```

## Encapsulation

```php

class Person
{

  public function __construct($name)
  {
    $this->name = $name;
  }

  public function job()
  {
    return 'software developer';
  }

  public function favouriteTeam()
  {
    //
  }

  private function thingsThatKeepUpAtNight()
  {
    return 'We are all going to die and that is terrifying.';
  }

  }

$bob = new Person('Bob');

```
* Public - Open to everyone.
  * Methods and property can be set to public.
  * Defaults to public.
  * Because the value is public, it can be changed outside the class.
  * There are situations where you might want to make a method public for internal reasons.
    * However, the outside world doesn't need to be calling this method.
    * Developers will often add a tag if this is the case.
* Private - This method is private to me.
  * Can only be accessed within the class.
  * It isn't full restriction, and can be exposed. It is more of a signal of what we intend to keep private.
* Protected - This class can access this method, as can the children that extend from this class.
* We should never be able to force an object into an invalid state.
* General rule of thumb - default to private, then protected, then public.

```php

//Add a getter

protected $playerOne;

public function playerOne()
{
  return $this->playerOne;
}

```

## Object Composition and Abstractions

* Combining types to build up a more complex object.
  * When one class has a pointer to another class where behaviour is located.

```php


class Subscription
{
  protected Gateway $gateway

  public funciton __construct(Gateway $gateway)
  {
    $this->gateway = $gateway
  }

  public function cancel()
  {
    $this->gateway->findStripeCustomer();
  }
}

interface Gateway
{
  public function findCustomer();
  public function findSubscriptionByCustomer();
}

class StripeGateway implements Gateway
{
  public function findCustomer()
  {

  }

  public function findSubscriptionByCustomer()
  {

  }
}

new Subscription(new StripeGateway());


```
* In this example, subscription doesn't care about the billing provider you use.
  * However, at the same time, it would be weird to have the `findCustomer()` function in subscription, as it isn't related to one inidividual customer and needs to be reusable.

## Value Objects and Mutability

* A value object is an object whose equality is determined by its data (or value) rather than any particular identity.

```php

//Rather than:

function register(string $name, int $age)
{
  if ($age < 0 || $age > 120) {
    throw new InvalidArgumentException('That makes no sense');
  }
}

register('John Doe', 500);

//Use:

class Age
{
  private $age;

  public function __construct($age)
  {
    if ($age < 0 || $age > 120) {
      throw new InvalidArgumentException('That makes no sense');
    }

    $this->age = $age;
  }
}

function register(string $name, Age $age)
{
}

register('John Doe', new Age(500));

```
* In the lower example, age is protecting its consistency.
* A mutable object is an object whose internal state can be change.
* Benefits of value objects:
  * It avoids primitive obsession and readability.
    * An age isn't just an integer but within
  * It helps with consistency.
  * By avoiding setters and setting to private you gain immutability.
* Only reach for a value object when it has benefits.

```php

class Coordinates
{
  private $x;
  private $y;

  public function __construct($x, $y)
  {
    $this->x = $x;
    $this->y = $y;
  }

}

//in situations like this, you need both values
function distance(Coordinates $begin, Coordinates $end)
{
  $coordinates->x;
}

```

## Exceptions

* Exceptions are the errors that get thrown when something goes wrong.
* You could type-set to trigger an error `function add(int $one, int $two)`
* You could use an if in the function to check the data.
  * This produces an uncaught error
* Or, in this circumstance, it would bubble up the chain to a gloabal exception handler.:

```php
function add($one, $two)
{
  if (! is_float($one) || ! is_float($two)) {
    throw new InvalidArgumentException('Please provide a float.');
  }

  return $one + $two
}

try {
  echo add(2,[]);
} catch (InvalidArgumentException $e) {
  echo 'Oh well.';
}
```
* Always ask yourself why you are throwing an exception.
  * It might just be that there is only a top-level exception rather than the others.

### Custom Exception Types

```php

<?php

class Member
{
  
  public $name;

  public function __construct($name)
  {
    $this->name = $name;
  }
}

class Team
{
  protected $members = [];

  public function add(Member $member)
  {
    if (count($this->members) === 3){
      throw new Exception('You may not add more than 3 team members.');
    }
    $this->members[] = $member;
  }

  public function members()
  {
    return $this->members;
  }
}

$team = new Team; // has a maximum of three members.
$team->add(new Member('Jane Doe'));

var_dump($team->members()); // Jane Doe, Frank Doe, John Doe, Sarah Doe

```
* Always want to protect ourselves at the lowest level.
* In situations where you have a clear business rule or requirement that is being broken, you may create a custom exception.
  * Stick to these situations otherwise you will end up creating custom exceptions for everything.

```php

class MaximumMembersReached extends Exception
{

}

...
 throw new MaximumMembersReached('You may not add more than 3 team members.');

...

```

* You can store the exception on the class itself or create a static constructor on the exception class.
