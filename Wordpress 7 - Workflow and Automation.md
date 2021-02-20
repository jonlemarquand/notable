# Wordpress 7 - Workflow and Automation

```php 

if (strstr($_SERVER['SERVER_NAME'], 'fictional-university.local')) {
  wp_enqueue_script('main-university-js', 'http://localhost:3000/bundled.js', NULL, 1.0, true);
} ekse {
  //load ordinary css/js
}

```