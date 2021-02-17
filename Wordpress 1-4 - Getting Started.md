# 2. Getting Started

## First Taste of PHP

# 3. First Coding Steps: PHP

## Creating a New Theme

* Themes need two files: `index.php` and `style.css`
* In wordpress, the style.css file is where we find info about the theme.
* Add screenshot.png for background.

``` css

/*
  Theme Name: Fictional Title
  Author: Jon Le Marquand
  Version: 1.0
*/

```

## PHP Functions

```php 

<?php
  function greet() {
    echo "<p>Hi, my name is $name and my favourite colour is $color.</p>";
  }

  greet('Jon', 'purple');
  greet('Ralph', 'blue');

?>

<h1><?php bloginfo('name'); ?></h1>
<p><?php bloginfo('description'); ?></p>

```
* The bloginfo('name') will output the site title from a wordpress function.

## PHP Arrays

* An array is a collection - "Available in a wide array of colours"

```php 

<?php 

  $names = array('Jon', 'Ralph', 'Squish');
  
  $count = 0
  while($count < count($names)) {
    echo "<p>Hi, my name is <?php echo $names[$count] ?></p>";
    $count++
  }
?>

```

* Wordpress uses while loops rather than for loops

# 4. Wordpress Specific PHP

## The Famous "Loop" in Wordpress

```php

<?php 

  while(have_posts()) {
    the_post(); ?>
    <h2><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
    <?php the_content(); ?>
    <hr>
    <?php
  }

?>

```

* Wordpress function `have_posts` will run once for each blog post.
* `the_post` function will get all the information about the current post.
* Create a new file: `single.php`
  * Depending on the current url, wordpress will look for different files in our theme folder.
  * This so far, has been posts.

### Pages

* Wordpress only uses `single.php` for posts, and `page.php` for pages.

## Header & Footer

* Create a new file: `header.php`
* In index/other files use: `get_header();`
* For the footer.php file you can use `get_footer();`
* In Wordpress you use `wp_head();` for css files.
* To load css we create a functinos.php file.
  * A behind the scenes file to have a conversation with wordpress

```php 

<?php

function university_files() {
  wp_enqueue_style('university_main_styles', get_stytlesheet_uri());
  wp_enqueue_script($arg, $location, $dependencies, 1.0, TRUE) //for js the true loads at the header
}

add_action('wp_enqueue_scripts', 'university_files');

>

```

* wp_enqueue_scripts is for js/css files - right before you output the header code, tack on this custom function.
* For the footer you want to add `<?php wp_footer() ?>`

## Convert Static HTML Templates into WordPress

* `get_theme_file_uri('/images/library-hero.jpg')` - This will generate the right theme url.