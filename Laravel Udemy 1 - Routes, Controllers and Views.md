---
title: 'Laravel Udemy 1 - Routes, Controllers and Views'
created: '2020-11-17T21:32:47.833Z'
modified: '2020-11-17T21:57:56.909Z'
---

# Laravel Udemy 1 - Routes, Controllers and Views

* For different ports use `php artisan serve --8000`
* Could use Valet on mac to automatically refresh.
* Routes are stored (as of Laravel 7) in routes > web.php rather than app > routes

## Laravel Structure Overview

* App > Models & Controllers
* Bootstrap - Folder where stuff starts
* Config - Where config files are located.
* Database - Where you create tables and migrations etc.
* Public - Public files such as css/js
* Resources - Files that need to be compiled are.
  * Using webpack
  * Also store views in the resources section.
* Routes
  * Where routes are located.
* Storage
* Tests
* Vendor
  * Where the applications and packages will be installed.
* .env is where we keep sensitive information.
* artisan file is what we use to make php stuff.

## Routes

```php

Route::get('/', function() {
  return view('welcome');
});

// You can pass multiple variables.

Route::get('/post/{id}/{name}', function($id, $name){
  return "This is post number " . $id . $name;
});

```
### Naming Routes

```php

Route::get('/admin/posts/example', array('as'=>'admin.home', function() {
  $url = route('admin.home');

  return "This url is " . $url;
}));

```
* What this means is that 'admin.home' will be the same thing as the url.
  * As demonstated by `php artisan route:list`
  * You could use `<a href="route('admin.home')">Click here</a>` for example.
