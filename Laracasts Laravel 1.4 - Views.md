---
title: Laracasts Laravel 1.4 - Views
created: '2020-08-01T19:19:57.159Z'
modified: '2020-08-02T08:59:23.631Z'
---

# Laracasts Laravel 1.4 - Views

## Layout Pages

* The layout files should have the wrapping HTML, rather than the content.
* In `welcome.blade.php` say:

```php

@extends ('layout')

@section('content')
//Insert content of the page here.

@endsection

```
* In `layout.blade.php` say `@yield ('content')`
* This makes it super easy to add scripts and other repeatable content.

## Set an Active Menu Link

```php

<ul>
  <li class="{{ Request:path() === '/' ? 'current_page_item' : ''}}">Homepage</li>
  <li class="{{ Request:path() === 'about' ? 'current_page_item' : ''}}">About Us</li>
</ul>

// Could also use
  <li class="{{ Request::is('about') ? 'current_page_item' : ''}}>About</li>
```

## Asset Compilation with Laravel Mix and webpack

* Vanilla css and js - Public directory,
* Sass and other js - Resources for build process compiled to the public directory.
* Laravel Mix is a wrapper around webpack that helps speed up the process.
  * For example, sass compilation: `npm install` > `npm run development`
    * This will compile the css.
  * Quicker way: `npm run watch` - It will now keep an eye on the mix files declared.

## Render Dynamic Data

* `php artisan make:model Article -m`

```php

...
  $table->bigIncrement('id');
  $table->string('title');
  $table->text('excerpt');
  $table->text('body');
...


//routes/web.php

Route::get('/about', function() {
  //order by created at column

  return view('about', [
    'articles' => App\Article::take(3)->latest()->get()
  ]);
});

//about view
<ul>
  @foreach ($articles as $article)
    <li>
      <h3>{{$article->title}}</h3>
      <p>{{$article->excerpt}}</p>
    </li>
  @endforeach
</ul>

```

### More Dynamic Data

```php

//routes

Route::get('/articles/{article}', 'ArticlesController@show');

//ArticlesController

public function show($id)
{
  $article = Article::find($id);

  return view('articles.show', ['article' => $article]);
}

//in show.blade.php

{{$article->title}}

```
* Sidenote: If you use relative paths in you can use either `"/css/file.css"` or `"{{asset}}css/defaults.css"`
