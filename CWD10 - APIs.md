---
title: CWD10 - APIs
created: '2020-02-24T07:49:01.000Z'
modified: '2020-02-24T12:39:05.888Z'
---

## What is an API?

* Your App --> Request --> Their App --> Data --> Your App
* You get a particular URL as a JSON object.
* To do this you also need an API key and if you copy and paste it into the URL in place of the default name.
* So for example:

```php
<?php

    $weather = "";
    $error = "";

    if ($_GET['city']) {

        $urlContents = file_get_content("http://api.openweathermap.org/data/2.5/weather?q=".$_GET['city'].",uk&appid=aawddawdadawfesfwe34");
        $weatherArray = json_decode($urlContents, true);
        $weather = "The weather in ".$_GET['city']." is currently '".$weatherArray['weather'][0]['description']."'. ";

    }


?>
```

## Google Maps API

### Geocoding with Google Maps

```javascript

var position = {};
$.ajax({
  url: past from google maps api,
  type: "GET",
  success: function(data) {
    $.each(data["results"][0], function(key,value) {
      if(value["types"][0] == "country") {
        alert(value["short_value"])
      }
    })
  }
})

```
