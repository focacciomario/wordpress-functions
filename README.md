# Wordpress Functions

A list of useful functions for wordpress that I use to clean up, customize and optimize my sites

- [Create Custom Wordpress Widget](#custom-wordpress-widget)
- [Remove all standard Wordpress widgets](#remove-all-standard-WP-widget)
- [Custom login logo](#custom-login-logo)
- [Disable xmlrpc.php](#disable-xmlrpc.php)
- [Remove Wordpress Admin Bar](#remove-wordpress-admin-bar)
- [Add Open Graph Meta Tags](#add-open-graph-meta-tags)
- [Load third-party scripts later to improve lighthouse performances](#load-third-party-scripts-to-improve-lighthouse-performances)




## Custom Worpress Widget
```php

/**
 * Custom WordPress dashboard widget
 */
 
function dashboard_widget_function() {
    echo '
        <h2>Custom Title Widget</h2>
        <p>Custom paragraph here</p>
        <img src='' width='240px' height='240px'>
    ';
}

function add_dashboard_widgets() {
    wp_add_dashboard_widget( 'custom_dashboard_widget', 'Custom Dashoard Widget', 'dashboard_widget_function' );
}
add_action( 'wp_dashboard_setup', 'add_dashboard_widgets' );
```


## Remove all standard WP widget
```php
/**
 * Remove all dashboard widgets
 */
 
function remove_dashboard_widgets() {
    global $wp_meta_boxes;
    
    unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_quick_press'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_incoming_links'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_right_now'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_plugins'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_drafts'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_comments'] );
    unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_primary'] );
    unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_secondary'] );

    remove_meta_box( 'dashboard_activity', 'dashboard', 'normal' );
}
add_action( 'wp_dashboard_setup', 'remove_dashboard_widgets' );
```

## Custom login logo
```php
/**
 * Inserire un logo personalizzato nella schermata di login wordpress
 */
 
function custom_login_logo() {
    echo '
        <style>
            .login h1 a { 
                background-image: url(./wp-content/uploads/folder_custom_logo/custom_logo.jpg) !important; 
                background-size: 234px 234px; 
                width:234px; 
                height:234px; 
                display:block; 
            }
        </style>
    ';
}
add_action( 'login_head', 'custom_login_logo' );
```

## Disable xmlrpc.php 

```php
/**
 * Disable xmlrpc.php
 */
 
add_filter( 'xmlrpc_enabled', '__return_false' );
remove_action( 'wp_head', 'rsd_link' );
remove_action( 'wp_head', 'wlwmanifest_link' );
```

## Remove Wordpress Admin Bar
```php
/**
 * Remove WordPress admin bar
 */

function remove_admin_bar() {
    remove_action( 'wp_head', '_admin_bar_bump_cb' );
}
add_action( 'get_header', 'remove_admin_bar' );

```


## Add Open Graph Meta Tags
```php
/**
 * Add Open Graph Meta Tags
 */

function meta_og() {
    global $post;

    if ( is_single() ) {
        if( has_post_thumbnail( $post->ID ) ) {
            $img_src = wp_get_attachment_image_src( get_post_thumbnail_id( $post->ID ), 'thumbnail' );
        } 
        $excerpt = strip_tags( $post->post_content );
        $excerpt_more = '';
        if ( strlen($excerpt ) > 155) {
            $excerpt = substr( $excerpt,0,155 );
            $excerpt_more = ' ...';
        }
        $excerpt = str_replace( '"', '', $excerpt );
        $excerpt = str_replace( "'", '', $excerpt );
        $excerptwords = preg_split( '/[\n\r\t ]+/', $excerpt, -1, PREG_SPLIT_NO_EMPTY );
        array_pop( $excerptwords );
        $excerpt = implode( ' ', $excerptwords ) . $excerpt_more;
        ?>
<meta name="author" content="Your Name">
<meta name="description" content="<?php echo $excerpt; ?>">
<meta property="og:title" content="<?php echo the_title(); ?>">
<meta property="og:description" content="<?php echo $excerpt; ?>">
<meta property="og:type" content="article">
<meta property="og:url" content="<?php echo the_permalink(); ?>">
<meta property="og:site_name" content="Your Site Name">
<meta property="og:image" content="<?php echo $img_src[0]; ?>">
<?php
    } else {
        return;
    }
}
add_action('wp_head', 'meta_og', 5);
```


## Load third-party scripts later to improve lighthouse performances
Aggiungi questo codice in app.js per migliorare il punteggio del sito Wordpress su Lighthouse o GTmetrix. È possibile modificare il tempo di timeout a piacimento, ma di solito 1000ms o 1s è un intervallo più che valido. 

```php
<script>
var fired = false;

window.addEventListener('scroll', () => {
    if (fired === false) {
        fired = true;
        
        setTimeout(() => {

            // Marketing scripts go here.

        }, 1000) // 1000ms or 1s works fine, but you can adjust this timeout.
    }
});
</script>
```

