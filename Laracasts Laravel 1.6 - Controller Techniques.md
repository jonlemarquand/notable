---
title: Laracasts Laravel 1.6 - Controller Techniques
created: '2020-08-02T19:02:04.721Z'
modified: '2020-08-02T19:26:00.604Z'
---

# Laracasts Laravel 1.6 - Controller Techniques

## Leverage Route Model Binding

* Use: `findOrFail` rather than `find`

```php

//Rather than use:
public function show($id)
{
  $article = Article::findOrFail($id);

  return view('articles.show', ['article' => $article]);
}

//We can use:
public function show(Article $article)
{
  return view('articles.show', ['article' => $article]);
}

```
* Laravel captures the wildcard and can use it.
  * However, it has to match up on the controller end in the input.
* If you don't want to use the primary key:

```php

//Article.php (model)
class Article extends Model
{
  public function getRouteKeyName()
  {
    return 'slug';
    // Article::where('slug', $article->first())
  }
}

```

## Reduce Duplication

```php

//Rather than:
public function store()
{
  $article = new Article();

  $article->title = request('title');
  $article->excerpt = request('excerpt');

  $article->save();

  return redirect('/articles');

}

//Use:
public function store()
{
  Article::create([
    'title' => request('title'),
    'excerpt' => request('excerpt'),
    'body' => request('body')
  ]);

  return redirect('/articles');

}

//Article.php

class Article extends Model
{
  protected $fillable = ['title', 'excerpt', 'body'];
}

// If there are no protected classes
class Article extends Model
{
  protected $guarded = [];
}

```

```php
//Rather than:
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

//Use:
public function store()
{
  
  Article::create(request()->validate([
    'title' => ['required', 'min:3', 'max:255'],
    'excerpt' => 'required',
  ]));

  return redirect('/articles');

}
```

## Consider Named Routes

* Named routes allow us to get around having to change the URL on every instance where it's hardcoded.

```php

//Routes:
Route::get('/articles/{article}', 'ArticlesController@show')->name('articles.show');

//Index.blade.php
<a href="{{ route('articles.show', $article) }}">

```
