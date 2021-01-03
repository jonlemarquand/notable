---
title: Laracasts Laravel 1.3 - Database Access
created: '2020-08-01T12:24:54.667Z'
modified: '2020-08-01T19:19:33.616Z'
---

# Laracasts Laravel 1.3 - Database Access

## Setup a Database Connection

* All important config info: `.env` file.
* In database.php file there will be a `'default' => env('DB_CONNECTION', 'mysql'),
  * This just tells the database to look for the connection in the env file, and if it can't find it, assume mySQL.

```php

// To do something with it
Route::get('/posts/{post}', function($slug) {
  
  $post = \DB::table('posts')->where('slug', $slug)->first();

  return view('post', [
    'post' => $post
  ]);
});

//
<p>{{ $post->body}}</p>

```
* The backslash is trying to access the DB class from the global namespace route.
  * It would assume the class was in the current namespace.
  * Or you can import it at the top.

## Hello Eloquent

* `php artisan make:model Post`

```php

use App\Post;

...
{
  $post = Post::where('slug', $slug)->firstOrFail();
}

```
* This removes the need for the abort if it is not found by the `OrFail()`

## Migrations 101

* `php artisan make:migration create_posts_table`
  * convention is `create__xxxtablename_table`
* Migration is like version control for your database.

```php

public function up()
{
  Schema::create('posts', function (Blueprint $table) {
    $table->bigIncrements('id');
    $table->string('slug');
    $table->text('body');
    $table->timestamps();
    $table->timestamp('published_at')->nullable();
  })
}

```
* nullable is the way of saying that a value is optional.
* The down method should include anyway of undoing the up methods.
* `php artisan migrate`

### How to add an additional column

```php
//Post-production method

$table->string('title');

...

$table->dropColumn('title');


//Pre-production method


```
* make the change in the original file and then php artisan migrate:rollback (which would lose all the data)
* If all the tables are empty `php artisan migrate:fresh`

## Generate Multiple Files in a Single Command

* `php artisan make:model Project -mc`
  * This single command gives us a migration, the project model and the controller for projects.

## Business Logic

* `php artisan make:model Assignment -mc`
  * Imagine this was something that a manager could assign to their staff.
  * `php artisan tinker` - You can practice a function call or resolve something out of the container.


```php

$assignment = new App\Assignment;

$assignment->body = 'Finish school work';

$assignment->save();

//This will return this value
$firstAssignment = App\Assignment::first(); 
App\Assignment::where('completed', false)->get();

$firstAssignment->complete()

//Assignment.php
class Assignment extends Model
{
  public funciton complete()
  {
    $this->completed = true;
    $this->save();
  }
}

```
* Using this code rather than directly is fairly procedural.
  * You don't want to repeat code, you want to expose a clean API that anyone can use.

