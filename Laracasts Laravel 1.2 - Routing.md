---
title: Laracasts Laravel 1.2 - Routing
created: '2020-07-29T08:27:44.713Z'
modified: '2020-08-01T11:38:29.786Z'
---

# Laracasts Laravel 1.2 - Routing

## Basic Routing and Views

* In routes if we go to: `routes/web.php`

```php

Route::get('/', function() {
  return view('welcome');
});

Route::get('/hi', function() {
  return 'Hello world'; // laravel would convert this to a useable string.
});

```

* This simply tells us to return the welcome page when we access the homepage.
* When creating views you can either use `test.php` or `test.blade.php`
  * Blade is a templating engine for laravel.

## Pass Request Data to Views

```php

Route::get('/', function() {
  $name= request('name');
  
  return view('test', [
    'name' => $name
  ]);
});

//test.blade.php

<body>
  <h1>{{ $name }}</h1>
</body>

```
* In the above example, if we now go to `http://127.0.0.1:8000/test?name=Jon`, the page would return Jon.
* However, this would leave an exploit that the user could echo anything they want.
* Laravel can use `{{ }}` to escape and convert the input to a string.

## Route Wildcards


```php
// To capture the route url wildcard
Route::get('/posts/{post}', function($post) {
  return $post;
});

// To do something with it
Route::get('/posts/{post}', function($post) {
  $posts = [
    'my-first-post' => 'Hello, this is my first blog post!',
    'my-second-post' => 'Now I am getting the hang of this blogging thing.'
  ];
  if (! array_key_exists($post, $posts)) {
    abort(404, 'Sorry, that post was not found.'); // Defaults to laravel's 404 page.
  }
  return view('post', [
    'post' => $posts[$post]
  ]);
});


```

## Routing to Controllers

```php

Route::get('/posts/{post}', 'PostsController@show');

//PostsController.php

class PostController
{
  public function show()
  {
    return 'Hello World!';
  }
}

```
* You can also use `php artisan make:controller PostsController`


