# Wordpress 8 - Events Post Type

## Custom Post Types

* A page is just a post with a special post type.
* In functions.php

```php 


function university_post_types() {
  register_post_type('event', array(
    'public' => true, //makes the post type visible to editors and the public
    'labels' => array(
      'name' => 'Events',
      'add_new_item' => 'Add New Event',
      'edit_item' => 'Edit Event',
      'all_items' => 'All Events',
      'singular_name' => 'Event'
    ),
    'menu_icon' => 'dashicons-calendar' // Look at wordpress dashicons
  ));
}

add_action('init', 'university_post_types');


```

* What would happen if we switched to a different theme?
  * You'd see an invalid post type. Events wouldn't be in the sidebar.
  * They are still in the database, but no way to access them.
* Instead of a theme functions.php file, this would be better by using must use plugins
* The folder in `wp-content/mu-plugins`.

```php 

//All the stuff from functions.php about post type

```

## Displaying Custom Post Types

```php 

$homepageEvents = new WP_Query(array(
  'posts_per_page' => 2,
  'post_type' => 'event'
));

while($homepageEvents->have_posts()) {
  $homepageEvents->the_post(); ?>
  <li><?php the_title(); ?></li>
  <?php } ?>
}

```
* Settings > Permalink Settings > Save - Will update the permalink structure for wordpress to register events.
* Create `single-event.php`
  * Wordpress will automatically look for single-custom-post-type.php
* In functions you need to set `has_archive => TRUE` in the custom post type.
* By default whatever keyword you use will be used for the slug, to change this use: `'rewrite' => array('slug' => 'events'),`
* You can also create `archive-event.php`
* In functions you might want to enable excerpts by: `'supports' => array('title', 'editor', 'excerpt')`
* To use block editor we need to use `'show_in_rest' => TRUE`