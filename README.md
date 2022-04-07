# wordpress-functions
A list of useful functions for wordpress that I use to clean up, customize and optimize my sites

[Create Custom Wordpress Widget](#custom-wordpress-widget)
[Remove all standard wordpress widgets](#Remove-all-standard-WP-widget)




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
