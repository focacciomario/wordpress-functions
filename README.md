# wordpress-functions
A list of useful functions for wordpress that I use to clean up, customize and optimize my sites

[Create Custom Wordpress Widget](#custom-wordpress-widget)





## Custom Worpress Widget
'''php
/**
 * Create custom WordPress dashboard widget
 */
 
function dashboard_widget_function() {
    echo '
        <h2>Custom Dashboard Widget</h2>
        <p>Custom content here</p>
    ';
}

function add_dashboard_widgets() {
    wp_add_dashboard_widget( 'custom_dashboard_widget', 'Custom Dashoard Widget', 'dashboard_widget_function' );
}
add_action( 'wp_dashboard_setup', 'add_dashboard_widgets' );
'''
