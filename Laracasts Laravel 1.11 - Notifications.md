---
title: Laracasts Laravel 1.11 - Notifications
created: '2020-08-20T06:56:18.769Z'
modified: '2020-08-21T05:45:46.021Z'
---

# Laracasts Laravel 1.11 - Notifications

## Database Notifications

* When something takes place in your system that should be passed on to the user, you could store it to a database and then display it on the website.

```php

php artisan notifications:table

php artisan migrate

//Every User model has a notifications

App/User::find(2)->notifications;

//In the controller

classUserNotificationsController extends Controller 
{
  public function show()
  {
    $notifications = auth()->user()->unreadNotifications;

    $notifications->markAsRead()

    return view('notifications.show', [
      'notifications' => $notifications
    ]);
  }
}
```

### The Tap Method

```php
public function show()
{
return view ('notifications.show', [
  'notifications' => tap(auth()->user()->unreadNotifications)->markAsRead()
]);
}
```
* This does a method on something, but does return anything extra.
  * The tap value is returned, and then you can do something on the object.

