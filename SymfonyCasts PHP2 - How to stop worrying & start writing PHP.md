---
title: SymfonyCasts PHP2 - How to stop worrying & start writing PHP
created: '2020-07-18T11:03:41.783Z'
modified: '2020-07-19T13:00:38.915Z'
---

# SymfonyCasts PHP2 - How to stop worrying & start writing PHP

## Reading POST'ed Data

* The `$_POST` variable is a superglobal.
  * POST is always available and equal to any submitted form data.
  * Each superglobal gives you information about the HTTP request our browser sends.

```php
if ($_SERVER['REQUEST_METHPD'] == 'POST') {
  if (isset($_POST['name'])) {
    $name = $_POST['name'];
  } else {
    $name = '';
  }

  $name = $_POST['name'];
  $breed = $_POST['breed'];
  $weight = $_POST['weight'];
}

```
* Any time you reference a key and an array, you have to ask yourself if it's possible the key and array doesn't exist.

## Saving Pets

```php

$pets = get_pets();
$newPet = array(
  'name' => $name,
  'breed' => $breed,
  'weight' => $weight,
  'bio' => $bio,
  'age' => '',
  'image' => '',
);
$pets[] = $newPet;

$json = json_encode($pets, JSON_PRETTY_PRINT);
file_put_contents('data/pets.json', $json);

```

## The Art of Redirecting

* You should redirect the user to a different page after a form submit.

```php

header('Location: /');

```

## Cleaning up with save_pets

```php

function save_pets($petsToSave) {
  $json = json_encode($petsToSave, JSON_PRETTY_PRINT);
  file_put_contents('data/pets.json', $json);
}

```

* Functions are reusable.
* The function name explains what it does.
