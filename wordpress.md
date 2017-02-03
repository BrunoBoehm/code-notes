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


# Customizing the Admin
## Colors
Using plugins
- admin color schemes
- admin color schemer

We can of course do it with code. We need a style.css and a function.php
In the wp-admin we can find the colors of the admin.

Let's namespace a function to add our color scheme to the admin dashboard using `wp_admin_css_color()`.
```php
<?php
    function bb_admin_color_scheme() {
        $theme_dir = get_stylesheeet_directory_uri();
        
        wp_admin_css_color(
            'treehouse', __( 'My Custom Scheme' ),
            $theme_dir . '/admin-colors/treehouse/colors.min.css',
            array( '#fff', '#fff', '#fff', '#fff' )
        );
    }
    
    add_action('admin_init', 'bb_admin_color_scheme');
?>
```
Then we command find replace using the code from an existing scheme.

## Login Page
The [login page customization](https://codex.wordpress.org/Customizing_the_Login_Form) works thanks to hooks, so everything happens in the `functions.php`.

It is also possible to do this using a wordpress plugin:
- custom login

## Admin menu
The owner doesn't need to access everything. User roles are important here.
- [removing menus](http://codex.wordpress.org/Function_Reference/remove_menu_page)
- [adding menus](http://codex.wordpress.org/Function_Reference/add_menu_page)

We can use code:
```php
<?php 
function custom_menu_page_removing() {
  remove_menu_page( 'index.php' );                  //Dashboard
  remove_menu_page( 'jetpack' );                    //Jetpack* 
  remove_menu_page( 'edit.php' );                   //Posts
  remove_menu_page( 'upload.php' );                 //Media
  remove_menu_page( 'edit.php?post_type=page' );    //Pages
  remove_menu_page( 'edit-comments.php' );          //Comments
  remove_menu_page( 'themes.php' );                 //Appearance
  remove_menu_page( 'plugins.php' );                //Plugins
  remove_menu_page( 'users.php' );                  //Users
  remove_menu_page( 'tools.php' );                  //Tools
  remove_menu_page( 'options-general.php' );        //Settings
}
add_action( 'admin_menu', 'custom_menu_page_removing' );
?>
```
Or use the plugin admin menu editor.

## Custom dashboard
Global option plugins to change the dashboard for allusers.

The is a [Dashboard widget API](https://codex.wordpress.org/Dashboard_Widgets_API), everything will happen again in our functions.php file thanks to `wp_add_dashboard_widget()`.
```php
function bb_add_dashboard_widgets() {
	wp_add_dashboard_widget(
                 'bb_welcome_dashboard_widget',         // Widget slug.
                 'Welcome to your site',                // Title.
                 'bb_dashboard_widget_function'         // Display function.
        );	
}
add_action( 'wp_dashboard_setup', 'example_add_dashboard_widgets' );

function bb_dashboard_widget_function() {
	echo "Hello World, I'm a great Dashboard Widget";
}
```

*Custom dahboard help widget* can be used to do this with a plugin`..

## Custom admin footer
We can use the *custom admin footer text plugin*
We can also to it with code in *functions.php*:
```php
<?php
    add_action('admin_footer', 'my_admin_footer_function');
    function my_admin_footer_function() {
	    echo '<p>This will be inserted at the bottom of admin page</p>';
    }

//or 

    add_filter('admin_footer_text', 'my_remove_footer_admin');
    function my_remove_footer_admin (){
        echo 'Custom HTML text';
    }
?>
```

# Wordpress Hooks
Hook - A generic term in WordPress that refers to **places where you can add your own code or change what WordPress is doing or outputting by default**. Two types of hooks exist in WordPress, actions and filters.
- [Action](http://codex.wordpress.org/Plugin_API/Action_Reference) - In WordPress is a hook that is triggered at specific time when WordPress is running and let’s you take an action. This can include things like creating a widget when WordPress is initializing or sending a Tweet when someone publishes a post.
- [Filter](http://codex.wordpress.org/Plugin_API/Filter_Reference) - In WordPress allows you get and modify WordPress data before it is sent from/to the database or the browser. Some examples of filters would include customizing how excerpts are displayed or adding some custom code to the end of a blog post.

- Query Monitor: this plugin will display all hooks and the actions that happen.

Don't hesitate to do a global doc search for `add_action( 'init` or `add_filter( 'wp_title` and see what code gets called for a few actions you're working with or debugging.

In the wp-settings of the core code we can see a lot of `do_action( 'init` or `apply_filters( 'wp_title` ... that's when the actions and filter get called.

It is a **best practice** to put your files in a modular way in folders, and then require them from your functions.php file with `require_once()`.
```php
<?php
    require_once('lib/custom_menu.php');
?>
```

> Making your code more modular by using actions and filter hooks is what your should aim for.

It can be interesting to `require_once( 'lib/export_wp_filter.php' );` and in this file have a `var_export()` function on the `$wp_filter` array:
```php
<pre>
    <?php var_export( $wp_filter ); ?>
</pre>
```
To make the array look a bit better, we can use [this code](http://www.rarst.net/wordpress/debug-wordpress-hooks/) it gives us a `list_hooks()` function. let's not forget to `include_once()` this file once downloaded. This is a good alternative to the [Query Monitor plugin](https://srd.wordpress.org/plugins/query-monitor/).

## filters
We can add something to filter hook
```php
function bb_excerpt_length( $length ) {
    return 16;
}

add_filter( 'excerpt_length', 'bb_excerpt_length', 999 );
```

We can remove something from a filter hook
```php
if ( has_filter( 'the_content', 'custom_plugin_function' ) ) {
    remove_filter( 'the_content', 'do_shortcode', 11 );
    add_filter( 'the_content', 'my_do_shortcode', 11 );
}
```

### Examples of filters
We can change the colmuns in the admin rea when listing posts
```php
<?php 
function manage_posts_columns_example( $columns ) {
	//if( $columns['title'] ) unset( $columns['title'] );
	unset( $columns['author'] );    
    unset( $columns['categories'] );
    unset( $columns['tags'] );
    unset( $columns['comments'] );
    return $columns;
}

add_filter( 'manage_posts_columns', 'manage_posts_columns_example' );
?>
```

We can add a custom class to the body depending on which page we are
```php
<?php
function custom_body_classes( $classes ) {
	if ( 'post' == get_post_type() ) {
		$classes[] = "custom-class";
	}	
    return $classes;
}

add_filter( 'body_class', 'custom_body_classes' );
?>
```

We can make a custom title for our pages
```php
<?php 
function custom_wp_title( $title, $sep ) {
	global $page;
	$title .= get_bloginfo( 'name' );
	$site_description = get_bloginfo( 'description', 'display' );
	
	if ( $site_description && ( is_home() || is_front_page() ) ) {
		$title = "$title $sep $site_description";
	} 
	return $title;
} 
add_filter( 'wp_title', 'custom_wp_title', 20, 2 );
?>
```

## Actions
### Adding an action to a hook
`<?php add_action( $hook, $function_to_add, $priority, $accepted_args ); ?>`
Let's add a script by hooking into the `wp_enqueue_scripts` action
```php
<?php 
function add_google_font() {
	wp_enqueue_style( 'google_font', 'http://fonts.googleapis.com/css?family=Pacifico' );
}
add_action( 'wp_enqueue_scripts', 'add_google_font' );
?>
```
Note that for a `add_filter` we had to return a value, here we're not, we just run code.

### Removing an action from a hook
`<?php remove_action( $tag, $function_to_remove, $priority ); ?>`
Let's now remove this action. This can be applied to any function that gets called on any hook, that we want to override.
```php
<?php 
    remove_action( 'wp_enqueue_scripts', 'add_google_font' );
?>
```

### The `do_action`
We can [define](http://wpengineer.com/1302/define-your-own-wordpress-hooks/) our own wordpress hooks!
```php
<?php
    function custom_footer() {
        do_action('my_footer');
    }

    function custom_footer_text() {
        echo "Custom footer text";
    }
    add_action( 'my_footer', 'custom_footer_text' );
?>
``` 

The in our index.php file for instance we can call the <?php custom_footer(); ?>. We just created a hook. The difference with `apply_filer()` is that here we don't need a default value (filter need to have values passed into them that they can output). There is no default action for a `do_action`.

We can check to see if a hook exist.
```php
<?php
    if ( has_action( 'init', 'custom_plugin_code' ) ) {
        remove_action( 'init', 'custom_plugin_code' );
        add_action( 'init', 'my_content_code' );
    }
?>
```

### Examples
Let's add our js and css files
```php
<?php 
    function theme_styles() {
    	wp_enqueue_style( 'bootstrap_css', get_template_directory_uri() . '/css/bootstrap.min.css' );
    	wp_enqueue_style( 'main_css', get_template_directory_uri() . '/style.css' );
    }
    add_action( 'wp_enqueue_scripts', 'theme_styles' );

    function theme_js() {
    	wp_enqueue_script( 'bootstrap_js', get_template_directory_uri() . '/js/bootstrap.min.js', array('jquery'), '', true );
    	wp_enqueue_script( 'theme_js', get_template_directory_uri() . '/js/theme.js', array('jquery', 'bootstrap_js'), '', true );
    }
    add_action( 'wp_enqueue_scripts', 'theme_js' );
?>
```

Let's register widgets
```php
<?php 
    function create_my_widget() {
        register_sidebar(array(
            'name' => __( 'My Sidebar', 'mytheme' ),    
            'id' => 'my_sidebar',
            'description' => __( 'The one and only', 'mytheme' ),
        ));
    }
    add_action( 'widgets_init', 'create_my_widget' ); 
?>
```

We can register a new menu
```php
<?php 
    function register_my_menus() {
      register_nav_menus(
        array(
          'footer_menu' => __( 'Footer Menu', 'mytheme' )      
        )
      );
    }
    add_action( 'init', 'register_my_menus' );
?>
```

We can remove menus from the admin area
```php
<?php 
    function wpt_remove_menus(){
      remove_menu_page( 'index.php' );                  //Dashboard
      //remove_menu_page( 'edit.php' );                   //Posts
      remove_menu_page( 'upload.php' );                 //Media
      //remove_menu_page( 'edit.php?post_type=page' );    //Pages
      remove_menu_page( 'edit-comments.php' );          //Comments
      
      //remove_menu_page( 'themes.php' );                 //Appearance
      //remove_menu_page( 'plugins.php' );                //Plugins
      //remove_menu_page( 'users.php' );                  //Users
      //remove_menu_page( 'tools.php' );                  //Tools
      //remove_menu_page( 'options-general.php' );        //Settings
    }
    add_action( 'admin_menu', 'wpt_remove_menus' );
?>
```

## Hooking to plugins
Plugins need to have hooks, and they need to be documented.

Let's change the class and submit button text of a gravity form (their hooks are well documented)
```php
add_filter( 'gform_submit_button', 'form_submit_button', 10, 2 );
function form_submit_button( $button, $form ) {
    return "<button class='button' id='gform_submit_button_{$form['id']}'><span>Submit</span></button>";
}
```

































