#Wordpress

## Creating a theme
`style.css` contains the meta information for the theme (and not styleS.css)
`functions.php` is not required to activate our theme
`index.php` is the main template
`screenshot.png` to display a picture

### Importing CSS
We can use the .css files of foundation and normalize.
We then use the functions.php to import

Don't use @import in your css file, make this from the functions.php file.
```php
<?php

function bb_theme_styles() {
  wp_enqueue_style( 'foundation_css', get_template_directory_uri() . '/css/foundation.css' );

  // wp_enqueue_style( 'normalize_css', get_template_directory_uri() . '/css/normalize.css' );

  wp_enqueue_style( 'googlefont_css', 'https://fonts.googleapis..' );

  wp_enqueue_style( 'normalize_css', get_template_directory_uri() . '/css/normalize.css' );
  
  wp_enqueue_style( 'main_css', get_template_directory_uri() . '/css/style.css' );
}

add_action( 'wp_enqueue_scripts', bb_theme_styles )

?>
```

### JS files
Wordpress ships with jQuery installed and running by default.
We can add
- our custom app.js file
- foundation.min.js
- modernizr.js
into our theme's /js directory

```php
<?php

function bb_theme_scripts() {
  wp_enqueue_script( 'modernizr_js', get_template_directory_uri() . '/js/modernizr.js', '', '', false );
  
  wp_enqueue_script( 'foundation_js', get_template_directory_uri() . '/js/foundation.min.js', array( 'jquery' ), '', true );
  
  wp_enqueue_script( 'main_js', get_template_directory_uri() . '/js/app.js', array( 'jquery', 'foundation_js' ), '', true );
}
// options of this function
// 1; array of dependence
// 1; specific version
// 3; we want this in the footer (true) or header (false)

add_action( 'wp_enqueue_scripts', 'bb_theme_js')
// wordpress, when you call wp_enqueue_scripts, call my bb_theme_js function

?>
```
We could also link from the header to all files directly, but what a mess.

Important: wordpress can work woth other libraries than jQuery, thus a $() line can be misinterpreted, we need to witch the noConflict mode on.

```php
jQuery(document).ready(function($) {
    // all of the jquery code goes in there.
    $(document).foundation();
});
```

### header.php and footer.php
This is a naming convention

In our index file we can call the header and footer, using includes.
```php
<?php get_header(); ?>
// contains the <!doctype html><html><header></header> 

<h1>Index</h1>

<?php get_footer(); ?>
// footer to closing html
```

Inside of the head we can use several functions 
```php
<?php wp_title(); ?>

<?php wp_head(); ?>

<?php blog_info('url'); ?>
<?php blog_info('name'); ?>
```

Inside of the footer we can use several functions 
```php
<?php echo dat('Y'); ?>

<?php wp_footer(); ?>
// tells wordpress it's the place to output footer tags, and.. javascript
```

Make sure to erase all scripts since they are now loaded from our functions.php .

### Wordpress Loop
It is a if/else/while loop to display posts and pages.

```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>

    <h1><?php the_title(); ?></h1>
    <p><?php the_content(); ?></p>
    <p><?php the_permalink(); ?></p>
    
<?php endwhile; else : ?>

	<p><?php _e( 'Sorry, no posts matched your criteria.' ); ?></p>
	
<?php endif; ?>
```

Note: `the_permalink` (will print the permalink) ≠ `get_permalink` (to store the variable). You might need to use `echo get_permalink` to print out the value.

## Working with static pages
We'll create a page.php

We can also add custom templates, by naming it page-sidebar-left.php for instance to show it is a `page-`. The structure at the top will tell wordpress it's a template.
```php
<?php 
/* 
    Template Name: Sidebar Left
*/
?>
```

## Navigation
By default wordpress doesn't give [theme support](http://codex.wordpress.org/Function_Reference/add_theme_support) for [menu](http://codex.wordpress.org/Function_Reference/wp_nav_menu), let's add it in our [functions.php](http://codex.wordpress.org/Function_Reference/register_nav_menus) file.
```php
<?php 
    add_theme_support('menus');
    // we can also add support for post thumbnails
    // add_theme_support('post-thumbnails');
    
    function register_theme_menus() {
        register_nav_menus(
            array(
                'primary-menu' => __( 'Primary Menu' ),
                'secondary-menu' => __( 'Secondary Menu' )
            )
        );
    };
    
    add_action( 'init', 'register_theme_menus' );
?>
```
Then we can access it in the admin area.

Now that the menu is registered, we can add it in our header
```php
<?php 
    $defaults = array(
        'container' => false,
        'theme_location' => 'primary-menu',
        'menu_class' => 'no-bullet'
    );
    
    wp_nav_menu( $defaults );
?>
```
By default wordpress wraps the menu uls inside of a div element, if we don't want it we put false.

Note: if working with the admin bar, it can modify the appearance of the menu. It is good to add to 
```php
<body <?php body_class(); ?>>
```
This will add usefull classes to your body tag.

## Custom Post Types
We can add more than just pages and posts.
We want the functionnality to stay even if we change theme, so we'll use a plugin (plugins stay as themes change).

We'll use advanced custom fields and custom post types UI (CPT UI).

### WP QUERY
We can customize what posts/pages... are rendered on a page using [WP_query()](http://codex.wordpress.org/Class_Reference/WP_Query).

Let's work in a page-portfolio.php template
```php
<?php 
/* 
    Template Name: Portfolio Page
*/
?>

<?php get_header(); ?>

<?php 
    $args = array(
        'post_type' => 'portfolio'
    );
    $query = new WP_Query( $args ); 
?>

<?php if ( $query->have_posts() ) :  while ( $query->have_posts() ) : $query->the_post(); ?>
	    <h2><a href="<?php the_permalink(); ?>" ><?php the_title(); ?></a></h2>
	    <?php the_post_thumbnail('large'); ?>
	<?php endwhile; ?>

	<?php wp_reset_postdata(); ?>

<?php else : ?>
	<p><?php _e( 'Sorry, no posts matched your criteria.' ); ?></p>
<?php endif; ?>

```

We can create a single-portfolio.php file to make the single page for a portfolio (when viewing only one portfolio item), and remove template info since this is default template taxonomy. We can use `<?php the_field(); ?>` to display custom fields.
```php
<?php get_header(); ?>

<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>

    <h1><?php the_title(); ?></h1>
    <p><?php the_field('description'); ?></p>
    <p><?php the_permalink(); ?></p>
    <hr>
    <p>
        <?php previous_post_link(); ?> - <a href="<?php bloginfo('url') ?>/portfolio">Back to Portfolio</a> - <?php next_post_link(); ?>
    </p>
    
<?php endwhile; else : ?>

	<p><?php _e( 'Sorry, no posts matched your criteria.' ); ?></p>
	
<?php endif; ?>

<?php get_footer(); ?>
```

## Blog Homepage
We can use 
```php
<?php echo get_avatar( get_the_author_meta('ID'), 24 ); ?>
<?php the_author_posts_link(); ?>
<?php the_category(', '); ?>
<?php the_time('F j, Y'); ?>

<?php echo strip_tags( get_the_excerpt() ); ?>
// as opposed to 
<?php the_content(); ?>

<?php if ( get_the_post_thumbnail() ) : ?>
    <?php the_post_thumbnail('large'); ?>
<?php endif; ?>
```
"get the post" works better than "the post" here.

If we wanted to change the excerpt length, we could go in the functions.php
```php
function bb_excerpt_length( $length ) {
    return 16;
}

add_filter( 'excerpt_length', 'bb_excerpt_length', 999 );
```
999 is the priority, we want this to come after whatever could come before in the process of "filtering"/modifying the content to display.


### Add comments
It's as easy as using [comments_template()](http://codex.wordpress.org/Function_Reference/comments_template). Adding a comments.php would override. 

It's good to look at other themes and copy what they're doing/ how they are doing it.

## Front Page / Home Page
The front page is the root page.
The Home page is the blog welcome page.
If we have a front page it will use it, otherwise it will use directly the home page template.

Let's create an include (like a partial in ruby) to put some of code to be reusable elsewhere.

We can create a `content-portfolio.php` page where we put the custom loop. And then call it whenever necessary with (slugs parts as parameters):
```php
<?php 
    $num_posts = ( is_front_page() ) ? 4 : -1 ;
    // -1 will show all
    $args = array(
        'post_type' => 'portfolio',
        'posts_per_pag' => $num_posts
    );
    $query = new WP_Query( $args );     
?>

// page code

// and where you want to include the content-portfolio.php content:
<?php get_template_part('content', 'portfolio') ?>

```

## Adding widgets for the sidebar

### Registering widgets
Let's use the [register_sidebar()](https://codex.wordpress.org/Function_Reference/register_sidebar) helper method.
```php
function bb_create_widget( $name, $id, $description ) {
    register_sidebar( array(
        'name' => __( $name ),
        'id' => $id,
        'description' => __( $description ),
        'before_widget' => '<div class="widget">',
        'after_widget' => '</div>',
        'before_title' => '<h2 class="module-heading">',
        'after_title' => '</h2>'
    ));
}

bb_create_widget( 'Page Sidebar', 'page', 'Displays on the side of pages with a sidebar' );
bb_create_widget( 'Blog Sidebar', 'blog', 'Displays on the side of pages in a blog section' );
```

### Showing the widgets
Wordpress has a sidebar.php type that we can use as an include for our sidebar code, using `<?php get_sidebar(); ?>`, or for a specific sidebar like sidebar-page.php `<?php get_sidebar( 'page' ); ?>`.

```php
<?php if( !dynamic_sidebar( 'page' ) : ?>
    <h2 class="module-heading">Sidebar Setup</h2>
    <p>Set this up in admin widgets area</p>
<?php endif; ?>
```

## Using shortcodes
We can rely on foundation shortcodes for instance to.
If we change the theme and not use foundation anymore then it will not work anymore.
We can use the easy-foundation-shortcode pluggin.

For clients its worth thinking about
- what to do with shortcodes
- what to hard-code in templates

## Testing the Wordpress Theme
Wordpress has a [checklist](https://codex.wordpress.org/Theme_Development_Checklist) for testing.

It's also very usefull to navigate the site with debug mode ON.
Inside of wp-config.php find the line `define( 'WP_DEBUG', false);` and change it to true. Great to use in development environment.

The theme-check plugin is also very handy for revealing errors and pieces of advice.






