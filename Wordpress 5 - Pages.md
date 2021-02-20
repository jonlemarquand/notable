# Wordpress 5 - Pages

## Interior Page Template

* Add a page title with - `the_title()`
* Add content with `the_content()`
* In functions, we can generate an appropriate title tag for each page.

```php

function university_features() {
  add_theme_support('title-tag');
}

add_action('after_setup_theme', 'university_features')l

```

* Use `site_url()` in a href to link to the homepage

## Parent & Children Pages

* In Document > Page Attributes you can set a page as a child of another.
* Post ids - Each post/page has a unique post id. In our code we can find out info about it.
* `get_the_ID()` - gives us the id of the current page being used.
* `wp_get_post_parent_id();` - This will give the pages parents id.
* Combine the two to use: `wp_get_post_parent_id(get_theID())`;
  * This returns 0 if there isn't a parent.

```php

if (wp_get_post_parent_id(get_theID())) {
  //Breadcrumb box in the html.
}

```
* `get_the_title(post_id);` will return the title for the post_id'd page.
* Similarly `get_permalink(post_id)` will get the permalink.

## To Echo or Not To Echo

* Why do certain functions need echo?
* Some functions return a result rather than echo it.
  * In which case you could echo a function.
* In terms of wordpress, 
  * if a wordpress function has get_the_id, it doesn't output on to the page.
  * If a wordpress function has the_id, it will echo out on to the page.

## Menu of child page links

* `wp_list_pages()` - Outputs all pages in the wp site.
  * Needs an associative array e.g. `$animalSounds = array('cat' => 'meow', 'dog' => 'bark', 'pig' => 'oink');`
  * You can then use $animalSounds['dog'];

```php 

if ($theParent) {
  $findChildrenOf = $theParent;
} else {
  $findChildrenOf = get_the_ID();
}

wp_list_pages(array(
  'title_li' => NULL,
  'child_of' => $findChildrenOf
))

```

## Misc additions

* In the head tag use `<meta name="viewport" content="width=device-width, initial-scale=1">`
* Wordpress in the `<html>` tag can use `<?php language_attributes(); ?>`
* `<meta charset="<?php bloginfo('charset');?>">` - Sets the charset
* `<body <?php body_class(); ?>>` - This will give the body tag various classes assigned by wordpress

## Navigation Menus

* To add a menu location, go to functions then add:
  * `register_nav_menu('headerMenuLocation', 'Header Menu Location');`
  * This will then show in Appearance > Menus

```php 

function university_features() {
  register_nav_menu('headerMenuLocation', 'Header Menu Location');
}

add_action('after_setup_theme', 'university_features');

//In header.php

<?php
  wp_nav_menu(array(
    'theme_location' => 'headerMenuLocation'
  ))
?>
```

* To hardcode an active link:

```php 

<?php if (is_page('about-us')) echo "class='current-menu-item'">

```