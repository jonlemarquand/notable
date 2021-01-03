---
title: Laracasts Laravel 1.9 - Core Concepts
created: '2020-08-08T11:01:14.021Z'
modified: '2020-08-08T15:07:59.455Z'
---

# Laracasts Laravel 1.9 - Core Concepts

## Collections

* If you were to fetch all or many articles, in that case you will get a collection of articles.
* Collections come with a number of options:
  * `$tags->where('name', 'laravel')` gets the same result as `App\Tag::where('name', 'Laravel')->first()` but there is no database query being done. It's all being done in the collection instance.
* To create a collection from scratch:
  * `collect(['one', 'two', 'three']);`
  * You can also use `->flatten()` to compress a nested array.
* Filter, map, flatmap, where, merge are the common ones.
* Filter isn't destructive. It doesn't rewrite the original variable, so you have to save it to a new variable.
* All of these methods can be chained.
  * `$items->filter(function ($item) { return $item % 2 === 0; })->map(function ($item) { return $item * 3; });`
* Practical example:

```php

$articles = App\Article:all();

// To load all articles with associated tags:
// This eager loads the tags (and all info that's associated with it.)
$articles = App\Article::with('tags')->get();

//At this point to return a tag list, for tags that are used:
$articles->pluck('tags')->collapse()->pluck('name')->unique();
//To pseudo-flatten it:
$article->pluck('tags.*.name')->collapse()->unique();


```

## CSRF Attacks

* In a CSRF (Cross-site request forgery) attack, an innocent end user is tricked by an attacker into submitting a web request that they did not intend.
* An hostile attacker can add a request to their own site, such as in an <img src="http://differentwbesite.com/logout"> they can then log out the website.
* Laravel is doing CSRF verification.
* In a form for logout, we can do a hidden input `@CSRF` which will submit a form with the token.
* There is also CSRF middleware.
* Stripe Webhooks, for example, need an exception.

## Service Container Fundamentals

```php

<?php

namespace App;

class Container 
{
  protected $bindings = []

  public function bind($key, $value) {
    $this->bindings[$key] = $value;
  }

  public function resolve($key) {
    if (isset($this->bindings[key])) {
      return call_user_func($this->bindings[key]);
    }
  }
}

//routes.php

Route::get('/', function() {
  $container = new \App\Container();
  $container->bind('example', function () {
    // In a real example you might have to do a few more things here.
    return new \App\Example();
  });

  $example = $container->resolve('example');
  ddd($container);

});

```
* The basic idea is that you can bind something into the container, and then later you can resolve it out of the container.

## Automatically Resolve

* Laravel's container is actually the app itself.

```php

//routes.php

Route::get('/', function () {
  $example = resolve(App\Example::class);

  ddd($example);
});

```

* With Laravel, you don't have to delcare all the variables, it keeps looking for files that line up with the names provided.
* A binding resolution exception is when laravel is trying to resolve dependencies but can't work out how because you haven't been explicit with declarations.
* The typical place to store services is in: Providers > AppServiceProvider.php
* If you want a single resolve for container, rather than a new instance use `singleton` rather than `bind`

## Laravel Facades Demystified

```php

use Illuminate\Support\Facades\View;

class Pages Controller extends Controller
{
  public function home()
  {
    return View::make('welcome');
  }
}

```
* Most of the framework is accessible through Facades 
* Facades provide a static interface to underlying components in the framework.
  * They are a convenience you can reference without having to manually build up things.
* Calling a static method that changes state is widely frowned upon.
  * If it's globally accessible and you can change state, that is bad.
  * However, facades, although you're calling a static method, is entirely testable.

```php

public function home()
{
  return request('name');
  //OR
  return Request::input('name');
}

```
* On the model level, probably not using a lot of facades, but the recommendation is play around with it and you'll find your style.

## Service Providers

* A service provider provides a service to the framework. It might need to register keys in the service container or trigger some functionality after it has been booted up.
* Register - for registering keys into the service container.
* Boot - triggers after every service provider has been registered.
