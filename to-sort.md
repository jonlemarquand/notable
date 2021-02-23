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

## Custom Fields

* You could edit custom fields into the block, but that's a terrible user experience.
* Use 'advanced custom fields'
* To query it: `the_field('event_date');` or `get_field('event_date')`

## Ordering (Sorting) Custom Queries

```php

$homepageEvents = new WP_Query(array(
  'orderby' => 'title',
  'order' => 'DESC' // Descending by default
  'posts_per_page' => -1 // this will return everything that meets the query
  //or use
  'meta_key' => 'event_date',
  'orderby' => 'meta_value_num',
  'meta_query' => array(
    array(
      'key' => 'event_date',
      'compare' => '>=',
      'value' => date('Ymd'),
      'type' => 'numeric'
    )
  )
))

```

## Manipulating Default URL Based Queries

* Custom queries are right when the page wouldn't be doing it anyway.

```php

function university_adjust_queries($query) {
  // not in admin, will return true for events, only on main query not custom query
  if (!is_admin() AND is_post_type_archive('event') AND $query->is_main_query()) { 
    $query->set('meta_key', 'event_date');
    $query->set('orderby', 'meta_value_num');
  }
}

add_action('pre_get_posts', 'university_adjust_queries');

```

## Past Events Page

* create a file `page-past-events.php`
* In general `echo paginate_links()` only works with default query.
* We can use:

```php 
//in query we need to add:
'paged' => get_query_var('paged', 1), // gets the page from the url or fallback to 1

echo paginate_links(array(
  'total' => $pastEvents->max_num_pages
))

```

# Wordpress 9 - Program Post Type

## Creating Relationships Between Content

* Custom fields - Related Program(s), set the field type to Relationship
  * In filter by post type click program
  * Show this field if its event

## Displaying Relationships

```php 

$relatedPrograms = get_field('related_programs');
if ($relatedPrograms) {
  foreach($relatedPrograms as $program) {
    echo get_the_title($program) // each item is a wordpress post object so can be queried.
  }
}

```
* To reverse the link between the posts, we need to write a custom query.

```php 

'meta_query' => array(
  array(
    'key' => 'related_programs',
    'compare' => 'LIKE',
    'value' => '"'.get_the_ID().'"' // Wordpress stores these values in a non-array with quotes
  )
)
```

# Wordpress 10 - Professors Post Type

* `wp_reset_postdata();` - Call it after a custom query.
* When you call a custom query in WP, it hijacks the global `the_post` variable.

## Featured Image (Post Thumbnail)

* By default, themes don't support featured images.
* `add_theme_support('post-thumbnails')` - This adds it in to blogs but not custom post types.
* In the post type add `title, editor, thumbnail`.
* We can use `the_post_thumbnail()` - To render the image.
* Can also use `the_post_thumbnail_url()` to get the image url for src tags.
* In functions.php, we can tell wp to generate any size:

```php 

function university_features() {
  add_theme_support('post-thumbnails');
  add_image_size('sizeNickname', width, height, crop or not); // set crop or not to true or false
}

add_action('after_setup_theme', 'university_features');

```
* There is a custom plugin for 'regenerate thumbnails' if you add in custom sizes.
* To call one of the custom sizes use: `the_post_thumbnail('sizeNickname');`
* `manual image crop tomasz` - Allows you to manually crop the images how you want.

## Page Banner Dynmaic Background Image

* In wordpress, a post can only have one featured image.
* Instead, you need to use custom fields.

```php

pageBannerImage = get_field('page_banner_background_image');
echo pageBannerImage['sizes']['pageBanner'];

```

# Wordpress 11 - Cleaner Code

## Reduce Duplicate Code

```php 

//php logic here - args = null provides a default value.
function pageBanner($args = NULL) {
  $args['title']
  // If there is no title set, this will be the page title.
  if(!$args['title']) {
    $args['title'] = get_the_title();
  }

  //HTML Code for page banner
}

// In single-professor.php
pageBanner(array(
  'title' => 'This is the title';
));


```

## Reduce Duplication - "get_template_part()"

* Subfolder: template-parts > event.php
* `get_template_part('template-parts/event', 'keyword');`
  * This word then look for the file `event-keyword`
* Function vs template part
  * Flexible with inputs - function
  * Blob of html - template-part

# Wordpress 18 - User Roles and Permissions

* By default, wordpress offers five roles.
* To create a new role: recommended `members` by memberpress plugin
* By default, wordpress treats new post types as default blog posts.
* In the MU folder, we need to say `'capability_type' => 'event'`

```php 

register_post_type('event',  array(
  'capability_type' => 'event',
  'map_meta_cap' => TRUE // This orders and enfoces the ordinary user permissions.
))

```
* You can also combine roles: Event Planner + Campus Manager

## Open Registration

* Membership > Anyone can register - If the member plugin is there.

```php 

if(is_user_logged_in()) {
  <a href="<?php echo wp_logout_url(); ?>">
  <?php echo get_avatar(get_current_user_id(), 60);>
} else {
  //other options
}

// Redirect subscriber accounts out of admin and onto homepage
add_action('admin_init', 'redirectSubsToFrontend');

function redirectSubsToFrontEnd() {
  $ourCurrentUser = wp_get_current_user();
  
  if(count($ourCurrentUser->roles) == 1 AND $ourCurrentUser->roles[0] == 'subscriber') {
    wp_redirect(site_url('/'));
    exit;
  }
}

// Hide the wp-admin bar for subscribers
add_action('wp_loaded', 'noSubsAdminBar');

function noSubsAdminBar() {
  $ourCurrentUser = wp_get_current_user();
  
  if(count($ourCurrentUser->roles) == 1 AND $ourCurrentUser->roles[0] == 'subscriber') {
    show_admin_bar(false);
  }
}

```

### To Customise the Login Screen

* Use wp_login_url(); and wp_registration_url(); for login and registration links.

```php 

// Change logo to homepage url
add_filter('login_headerurl', 'ourHeaderUrl');

functino ourHeaderUrl() {
  return esc_url(site_url('/'));
}

add_action('login_enqueue_scrips', 'ourLoginCSS');

function ourLoginCSS() {
  wp_enqueue_style('our-main-styles', get_theme_file_uri('styles.css'));
}

add_filter('login_headertitle', 'ourLoginTitle');

function ourLoginTitle() {
  return get_bloginfo('name');
}

```
