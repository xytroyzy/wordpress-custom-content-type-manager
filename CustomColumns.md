# Introduction #

This page explains how to customize the columns that are displayed when you view the list of all your posts inside the WordPress manager, e.g. when you click "All Pages".

In order to use this feature, you must edit your post-type definition and check the "Customize Columns" checkbox.

![http://content.screencast.com/users/fireproofsocks/folders/Snagit/media/3f3c00fa-6844-47b5-8bac-bbde7672afbb/2013-04-08_15-40-28.png](http://content.screencast.com/users/fireproofsocks/folders/Snagit/media/3f3c00fa-6844-47b5-8bac-bbde7672afbb/2013-04-08_15-40-28.png)

# Limitations #

This is yet another area where WordPress has some strange limitations that prevent otherwise sensible usage.

  * Your post-type name must not contain hyphens.
  * You cannot override the content of built-in WordPress columns
  * It is recommended that you always include the post-title in your list of columns
  * If you display a custom field as one of your columns, the value displayed will be filtered using the default [Output Filter](OutputFilters.md) defined for that field.

# Usage #

Normally, you can rely on the GUI to draw the columns you wish to display.  Remember that custom fields will rely on any default output filter defined.  This is especially important if you are printing out things like a relation, image, or even a user field of some sort.

# Further Customizations #

As of version 0.9.7.1, you can register a callback function to draw the content of each column.

The name of the callback function needs to be in the following format:

```
cctm_custom_column_{$post_type}_{$fieldname}()
```

So for example, if you wanted to draw your own column for the "order" post-type and the "status" column, you would create a function named `cctm_custom_column_order_status`.  The function should print its data.

Here's how we might manually print out the contents of our order's status:

```
// In theme's functions.php file:
function cctm_custom_column_order_status() {
	global $post;
	print_custom_field('status');
}
```


Remember: the function is called merely because it exists.  This does not rely on the standard WordPress callback action/filter behavior.

See `includes/CCTM_Columns.php` and the `populate_custom_column_data()` function for an example.