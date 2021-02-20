# Wordpress 6 - Building the Blog Section

## Blog Listing Page (index.php vs. front-page.php)

* In settings you can set your front page displays and post page displays to any page you want.
* `front-page.php` is what will be used for the homepage.
* To output the blog posts: 

```php 

while(have_posts()) {
  the_post(); ?>
  <div class="post-item">
    <h2><?php the_title(); ?></h2>
    <p>Posted by <?php the_author_posts_link(); ?> on <?php the_time('j.n.y');?> in <?php echo get_the_category_list(', ');?></p>
    <?php the_excerpt(); ?>
  </div>
<?php 
  } 
  echo paginate_links();
?>

```
* By default, Wordpress will show the latest 10 blog posts.
* In settings, you can set the number of blog posts shown in reading.

## Archives

* Such as category, author etc
* If there is no file, they will be powered by 'index.php' but index is never wordpress's first choice.
* Instead, use `archive.php`
* Could use: `if (is_category()) { echo "category name will go here";}` or `is_author`
* To output a title: `single_cat_title();` or `the_author();`
* Key function: `the_archive_title();` will take care of everything but won't have fine grain control over the phrasing of the titles.
  * Can also use: `the_author_description();`

## Custom Queries

* So far Wordpress has automatically queried the right content.

```php 

$homepagePosts = new WP_Query(array(
  'posts_per_page' => 2
  'category_name' => 'award'
));

while ($homepagePosts->have_posts()) {
  $homepagePosts->the_post(); ?>
  <li><?php the_title() ?></li>
  <?php echo wp_trim_words(get_the_content(), 18); ?> // This will trim the first 18 words for the content.
<?php } wp_reset_postdata()?>

```
* wp_reset_postdata() will reset to before the custom query. Good for global variables etc.
