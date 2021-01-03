---
title: 'SymfonyCasts PHP7 - OOP2: Services, Dependency Injection and Containers'
created: '2020-07-25T09:18:17.413Z'
modified: '2020-07-25T12:50:12.401Z'
---

# SymfonyCasts PHP7 - OOP2: Services, Dependency Injection and Containers

## Service Classes

* The 'Ship' object in the example basically just holds data/state.
  * Also known as 'Models'
* The other big reason to create a class is to do some work.
  * Rather than creating a flat function with a calculation in it, create a class.
  * You can also put multiple methods in one class, if they are thematically similar.
  * BattleManager - a "service" class.

```php

class BattleManager
{
  public function battle() {
    ...
  }
}

require_once __DIR__ .'/lib/BattleManager.php';

$battleManager = new BattleManager();
$outcome = $battleManager->battle($ship1, $ship1Quantity, $ship2, $ship2Quantity);

```
* When we want to call a method on battle, we need a battle manager object.
* General tip: Start with private, only change to public if necessary.
  * Private lets us know it is only affected in the containing code.

### Sharpening the Battle Result with a Class

```php
<?php

class BattleResult
{
  private $usedJediPowers;
  private $winningShip;
  private $losingShip;

  public function __construct($usedJediPowers, Ship $winningShip, Ship $losingShip)
  {
    $this->usedJediPowers = $usedJediPowers;
    $this->winningShip = $winningShip;
    $this->losingShip = $losingShip;
  }

  public function getWinningShip()
  {
    return $this->winningShip;
  }

  public function wereJediPowersUsed()
  {
    return $this->usedJediPowers;
  }

}

// BattleManager.php

$battleResult = $battleManager->battle($ship1, $ship1Quantity, $ship2, $ship2Quantity);

$battleResult->getWinningShip()

```

### Objects are Passed by Reference

* It means that there is only 'Ship 1' in existance.
  * For arrays, if we changed a value, the original ship 1 would be the same.

## Fetching Objects from the Database

* At the root of the project create a `resources > init_db.php`

```php
private function queryForShips()
{
  $pdo = new PDO('mysql:host=localhost;dbname=oo_battle', 'root');
  $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  $statement = $pdo->prepare('SELECT*FROM ship');
  $statement->execute();
  $shipsArray = $statement->fetchAll(PDO::FETCH_ASSOC);

  return $shipsArray
}

public function getShips()
{
  $shipsData = $this->queryForShips();

  $ships = array();
  foreach ($shipsData as $shipData) {
    $ship = new Ship($shipData['name']);
    $ship->setWeaponPower($shipData['weapon_power']);

    $ships[] = $ship;
   }
}

```
* Moving things in to functions make it reusable, and gives it a name.


### Making only one DB connection with a Property

* Properties have only been on model classes so far.
* In service classes:
  * Hold options on class behaviour.
  * Holds tools (like a PDO)

```php
class ShipLoader
{
    private $pdo;
    private function getPDO()
    {
        if ($this->pdo === null) {
            $this->pdo = new PDO('mysql:host=localhost;dbname=oo_battle', 'root');
            $this->pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        }
        return $this->pdo;
    }
}
```

## OO Best Practice: Centralising Configuration

* Goal: Move the db configuration to a central location.
  * Don't just move the db config and set global keywords.

```php

private $dbDsn;
private $dbUser;
private $dbPass;

public function __construct($dbDsn, $dbUser, $dbPass)
{
  $this->dbDsn = $dbDesn;
  $this->dbUser = $dbUser;
  $this->dbPass = $dbPass; 
}

class ShipLoader
{
    private function getPDO()
    {
        if ($this->pdo === null) {
            $this->pdo = new PDO($this->dbDsn, $this->dbUser, $this->dbPass);
            $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        }
        return $this->pdo;
    }
}

// bootstrap.php

$configuration = array(
    'db_dsn'  => 'mysql:host=localhost;dbname=oo_battle',
    'db_user' => 'root',
    'db_pass' => null,
);

//index.php

require __DIR__.'/bootstrap.php';
$shipLoader = new ShipLoader(
    $configuration['db_dsn'],
    $configuration['db_user'],
    $configuration['db_pass']
);

```
* The Ship Loader now has a construct argument and private property rather than hard coded data.
* Don't put configuration inside a service class. Instead, use an argument.
  * This allows anyone using the class to pass in what they want.
  * This is called dependency injection.

## OO Best Practice: Centralising the Connection

* If you want to move something out of the service class, add it as a construct object and pass it in.

```php

class ShipLoader
{

    private $pdo;
    
    public function __construct(PDO $pdo)
    {
        $this->pdo = $pdo;
    }

    private function getPDO()
    {
        return $this->pdo;
    }
}

//index.php


$pdo = new PDO(
    $configuration['db_dsn'],
    $configuration['db_user'],
    $configuration['db_pass']
);
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
$shipLoader = new ShipLoader($pdo);

```
* Don't include configuration or create new service objects from within a service. 
  * Even though the PDO class comes from PHP, it is a service class: it does work. 
  * If we create that service object from within a class, we can't easily share it or control it.
* Instead, create all of your service objects in one place and then pass them into each other.
  * The downside is that the code to create the service objects is getting a bit complicated. And it's duplicated!

## Service Container

* In lib create a new file called `container.php`

```php

class Container
{
  private $configuration;

  public function __construct(array $configuration)
  {
    $this->configuration = $configuration;
  }

    /**
     * @return PDO
     */
    public function getPDO()
    {
        $configuration = array(
            'db_dsn'  => 'mysql:host=localhost;dbname=oo_battle',
            'db_user' => 'root',
            'db_pass' => null,
        );
        $pdo = new PDO(
            $this->configuration['db_dsn'],
            $this->configuration['db_user'],
            $this->configuration['db_pass']
        );
        $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        return $pdo;
    }
}

//index.php

$container = new Container($configuration);
$pdo = $container->getPDO();

//bootstrap.php

require_once __DIR__.'/lib/Container.php';

```

## Container: Force Single Objects

```php

class Container
{
    private $pdo;
    public function getPDO()
    {
        if ($this->pdo === null) {
            $this->pdo = new PDO(
                $this->configuration['db_dsn'],
                $this->configuration['db_user'],
                $this->configuration['db_pass']
            );
        $this->pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        }
        return $this->pdo;
    }


    private $shipLoader;
    /**
     * @return ShipLoader
     */
    public function getShipLoader()
    {
        if ($this->shipLoader === null) {
            $this->shipLoader = new ShipLoader($this->getPDO());
        }
        return $this->shipLoader;
    }

}

//index.php

$container = new Container($configuration);
$shipLoader = $container->getShipLoader();

```
* What are the benefits?
  * With the container object, none of the objects are created until they are called saving CPU and memory.
  * This is a dependency injection container.
    * A special class and you always have just one.
