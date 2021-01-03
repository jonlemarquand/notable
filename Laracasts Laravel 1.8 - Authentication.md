---
title: Laracasts Laravel 1.8 - Authentication
created: '2020-08-04T20:06:20.122Z'
modified: '2020-08-04T20:29:25.867Z'
---

# Laracasts Laravel 1.8 - Authentication

## Build a Registration System in Minutes

* `composer require laravel/ui --dev`
* `php artisan ui react --auth`
* Reference the auth like so:

```php

Route::get('/home', 'HomeController@index')
  ->name('home')
  ->middleware('auth');

```
### One content for users, one for unregistered

```php

<div class="content">
  @auth
    Hi, {{ Auth::user()->name }}
  @else 
    Laravel
  @endauth
</div>

```
* The opposite of @auth, to check if the user is signed in, use @guest.

## The Password Reset Flow

1. Click "Forgot Password"
2. Fill out a form with their email address
3. Prepare a unique token and associate it with the user's account.
4. Send an email with the unique link back to our site that confirms email ownership.
5. link back to the website, confirm the token, and set a new password.
