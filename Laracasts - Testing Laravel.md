---
title: Laracasts - Testing Laravel
created: '2020-11-16T22:05:59.905Z'
modified: '2020-11-16T22:27:31.766Z'
---

# Laracasts - Testing Laravel

* Everyone's testing, you just usually do it manually.
  * Take what you currently do, and find out how to automate it.
* Run `phpunit` or `php artisan test`

```php

//phpunit

public function testBasicExample()
{
  $this->visit('/')
      ->see('Laravel 5');
}

//Laravel 5 is the title of the page.

```
* Visit and see are Laravel specific wrappers for tests.
