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
  wp_enqueue_style( 'foundation_css', get_template_directory_uri() . '/css/foundations.css' );

  wp_enqueue_style( 'normalize_css', get_template_directory_uri() . '/css/normalize.css' );

  wp_enqueue_style( 'google_fonts', 'https://fonts.googleapis..' );

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
  
  wp_enqueue_script( 'foundation_js', get_template_directory_uri() . '/js/foundation.js', array( 'jquery' ), '', true );
  
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
