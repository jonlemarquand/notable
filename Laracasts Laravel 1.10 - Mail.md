---
title: Laracasts Laravel 1.10 - Mail
created: '2020-08-08T15:46:49.733Z'
modified: '2020-08-08T17:43:58.520Z'
---

# Laracasts Laravel 1.10 - Mail

## Send Raw Mail

```php

//routes.php
Route::get('/contact', 'ContactController@show');
Route::post('/contact', 'ContactController@store');

// ContactController

use Illuminate\Support\Facades\Mail;

class ContactController extends Controller
{
  public function show()
  {
    return view('contact');
  }

  public function store()
  {
    request()->validate(['email' => 'required|email']);
    $email = request('email');

    Mail::raw('It works!', function ($message) {
      $message->to(request'email'))
        ->subject('Hello there');
    });

    return redirect('/contact')
      ->with('message', 'Email sent!');
  }
}

//contact.blade.php
@error('email')
  <div class="text-red-500 text-cs">{{ $message }}</div>
@enderror

@if(session('message'))
  <div>
    {{ session('message') }}
  </div>
@endif

```
* The from email can either be defined in the config, or explicitly in the ContactController.store

## Send HTML Emails Using Mailable Classes

* `php artisan make:mail ContactMe`
* Views > Emails > contact-me.blade.php

```php

Mail::to(request('email'))
  ->send(new ContactMe());

```
* Any public properties value in the mailer class will be available in the view.

## Use Markdown to Compose Email

```php

return $this->markdown('emails.contact-me');

@component('mail::message')

#Standard Markdown Stuff

@component('mail::button', ['url' => 'https://laracasts.com'])
  Visit Laracasts
@endcomponent

@endcomponent

```
* General rule of markdown is don't indent, unless apparently in Notable.
* Loading component is similar to loading a view. But the colons :: signify a vendor directory.
* `php artisan make:mail Contact --markdown=emails.contact` - This will automatically create a markdown file along with the mail contact.
* But what if you want to cusotmise the vendor assets?
  * Use `php artisan vendor:publish` this will publish the assets in to your resources directory.
  * Check the documentation for what tag to use, to only publish certain packages.

## Notifications vs Mailables

* As a general rule of thumb, you are notifying the user in response to some action that took place on the website.
* They made a payment, they liked something etc.
* The way you notify them can take many forms.

```php

public function store()
{
  request()->user()->notify(new PaymentReceived());
  Notification::send(request()->user(), new PaymentReceived());
}

```
* You can have one notification that can be distributed in multiple channels.
