---
title: Laracasts Laravel 1.5 - Forms
created: '2020-08-02T09:10:54.978Z'
modified: '2020-08-02T17:21:29.575Z'
---

# Laracasts Laravel 1.5 - Forms

## The Seven Restful Controller Actions

* Index - a list of _____ (users/projects)
  * Renders a list of a resource
* Show - a specific _____ (user/project etc)
  * Show a single resource
* Create
  * Shows a view to create a new resource
  * A form to fill out new info
* Store (Persist (store/save))
  * A way to handle the above submission
* Edit
  * Show a view to edit exisiting item in a form.
* Persist the edit (update)
  * Save the edit
* Destroy (delete)

* So many projects are a repetition of CRUD items.
* You can use `php artisan make:controller ProjectsController -r` to create all 7 actions.
  * Adding the `-m` in as well it will add the Project model in. (Showing the $project as an object to pass in.)

## RESTful Routing

* Everything you need to know should be part of the request, a stateless request.
* As a general rule of thumb, don't put verbs in your url, use HTTP terms: GET, POST, PUT, DELETE etc.
  * `PUT /articles/:id` rather than `/articles/:id/update`

## Form Handling

* In routes, order matters:
  * If there's a wildcard above a later route, it will always follow the wildcard.

```php

//ArticlesController.php

public function create()
{
  return view('articles.create');
}

public function store()
{
  $article = new Article();

  $article->title = request('title');
  $article->excerpt = request('excerpt');

  $article->save();

  return redirect('/articles');

}

// create.blade.php

@extends('layout')

@section
  <div id="wrapper">
    <div id="page" class="containter">
      <h1>New Article</h1>
      <form method="POST" action="/articles">
        @csrf
        <div class="field">
          <label class="label" for="title">Title</label>
          <div class="control">
            <input class="input" type="text" name="title" id="title">
          </div>
        </div>
      </form>
    </div>
  </div>

@endsection
```

## Forms that submit PUT requests

* Modern browsers only like GET and POST so we can use:

```php

<form method="POST" action="/articles/{{ $article->id }}">
  @csrf
  @method('PUT')

```

## Form Validation Essentials

* As a general rule-of-thumb always presume user data is malicious.

```php

public function store()
{
  request()->validate([
    'title' => ['required', 'min:3', 'max:255'],
    'excerpt' => 'required',
  ]);


  $article = new Article();
  $article->title = request('title');
  $article->excerpt = request('excerpt');

  $article->save();

  return redirect('/articles');

}

//create.blade.php
//helper message
<input class="input @error('title') is-danger @enderror" type="text" name="title" id="title" value="{{ old('title') }}">
@if ($errors->has('title'))
  <p class="help is-danger">{{ $errors->first('title') }}</p>
@endif

```
* You can also use browser-level validation. Good practice to use both.
