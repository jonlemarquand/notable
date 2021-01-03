---
title: Laracasts Laravel 1.7 - Eloquent
created: '2020-08-03T18:35:16.941Z'
modified: '2020-08-04T17:01:28.957Z'
---

# Laracasts Laravel 1.7 - Eloquent

## Basic Eloquent Relationships

```php

//$user->projects
//$project->user

//Project.php

class Project extends Model
{
  public function user()
  {
    return $this->belongsTo(User::class);
    // select * from user where project_id = current user instance
  }
}

$project->user

// User.php

...
return $this->hasMany(Project::class, __);
// select * from articles where user_id = current user instance
// value 2 in the brackets will override the foreign key.

```
* To make this work, we need links in the database where projects and articles have a user id.
* belongsTo, hasOne, hasMany are all options.

## Understanding Foreign Keys and Database Factories

* Database factories are a way of producing dummy data.

```php
php artisan tinker

//generate unique set of dummy data and persist it.
factory(App\User::class, 5)->create();

php artisan make:factory ArticleFactory -m "App\Article"

$factory->define(Article::class, function (Faker $faker){
  return [
    'user_id' => factory(\App\User::class),
    'title' => $faker->sentence,
    'excerpt' => $faker->sentence,
    'body' => $faker->paragraph
  ];
});

```
* But what about if we delete the user_id? The articles would still be referencing it. Instead:

```php

//create_articles_table.php

public function up()
{
  Schema::crea('articles', function (Blueprint $table) {

  ...

  $table->foreign('user_id')
    ->references('id')
    ->on('users')
    ->onDelete('cascade');
  });
}

```
* This is laravel's blueprint syntax for assigning foreign keys and the data along with it.
* Now if you delete a user, it will delete all associated articles with it.

## Many to Many Relationships with Linking Tables

* What do we do if an article has tags... many tags.
  * A tag doesn't really belong to an article.
  * An article can have many tags and a tag can have many articles.
* To make it work, we need 3 tables:
  * Articles, tags and an intermediate table (A Pivot table, a linking table)
  * Convention for a pivot table is to take two tables, 'articles' and 'tags', make them singular, separate them by an underscore and arrange them in alphabetical order: 'article_tag'

```php

Schema::create('article_tag', function (Blueprint $table) {
  $table->bigIncrements('id');
  $table->unsignedBigInteger('article_id');
  $table->unsignedBigInteger('tag_id');
  $table->unique(['article_id', 'tag_id']);

  $table->foreign('article_id')->references('id')->on('articles')->onDelete('cascade');
  $table->foreign('tag_id')->references('id')->on('tags')->onDelete('cascade');
});

//Now:

$article = App\Article::first();
$article->tags->pluck('name'); //Will return 3 tags.

```

### Display All Tags Under Each Article

```php

//index.blade.php

@forelse ($articles as $article)
  <div class="content">
    <div class="title">
      <h2>
        <a href="{{ $article->path() }}">
          {{ $article->title }}
        </a>
      </h2>
    </div>
  </div>
@empty
  <p>No relevant articles yet.</p>
@endforeach

//ArticlesController.php

public function index()
{
  if(request('tag')) {
    $articles = Tag::where('name', request('tag'))->firstOrFail()->articles;
  } else {
    $articles = Article::latest()->get();
  }

  return view('articles.index', ['articles' => $articles]);
}

```

## Attach and Validate Many-to-Many Tags

```php

//create.blade.php
<select
  name="tags[]"
  multiple
>
  @foreach ($tags as $tag)
    <option value="{{ $tag->id }}">{{ $tag->name }}</option>
  @endforeach
</select>
//ArticlesController.php

public function create()
{
  return view('articles.create', [
    'tags' => Tag::all()
  ]);
}

public function store()
{
  $article->tags()->attach(request('tags'));
}

//simple: $article->tags()->attach([1,2]);

//to validate:

protected function validateArticle()
{
  return request()->validate([
    ...
    'tags' => 'exists:tags,id'
  ]);
}
```
