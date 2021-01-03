---
title: 262-263 - HTML5 Features
created: '2019-09-22T19:55:13.534Z'
modified: '2019-09-22T20:29:41.275Z'
---

# 262-263 - HTML5 Features

## HTML Form Features

* First thing to use is `placeholder=""` which allows you to use placeholder text in a form.
* `autofocus` in an input tag will draw the focus to that form element when the page loads.
* `autocomplete="off"` will stop autocompleting
  * useful for credit card numbers etc
* `required` forces a certain field to be filled in before you can submit.
* `input type="email"` forces the text in the field to have an @ in email format.
* `pattern=""` is useful for HTML validation without needing javascript
* To link code outside a form, to the form you can use an id on the form and then link it with `form="myID"` on the element itself.
* To submit multiple files you can use `<input type="file" multiple>

## Audio & Video

To show video in HTML5:

```html

<video width="320" height="240" controls autoplay>
  <source src="myvideo.mp4" type="video/mp4">
  Please upgrade your browser.
</video>

```

To show audio in HTML5:

```html

<audio controls autoplay>
  <source src="audio.mp3" type="audio/mpeg">
  Please upgrade your browser.
</video>

```
