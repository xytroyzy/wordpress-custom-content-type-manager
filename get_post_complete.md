# get\_post\_complete #

_array_ **get\_post\_complete**( integer _$id_ )

This gets a _complete_ post as an associative array, including all custom fields.  REMEMBER: the data is returned as an ARRAY, not as an OBJECT!  You must access properties as `$p['property']` and not as `$p->property`

### $id ###
(integer) (required) The ID of the post whose data you want.

## Example ##

```
<?php $my_post = get_post_complete(123); ?>
<a href="<?php print $my_post['guid']; ?>"><?php print $my_post['post_title']; ?></a>
<?php print $my_post['my_custom_field']; ?>
```

PHP's [print\_r()](http://php.net/manual/en/function.print-r.php) function is your friend here: use it to see what fields are available for each post.

## Example2 ##

You can also use this function to help grab all of a post's custom fields in a loop:

```
// Note that WP's get_posts function returns OBJECTS, not arrays!
$posts = get_posts(array('post_type'=>'book'));

foreach($posts as $p) {
	$pc = get_post_complete($p->ID);
        // get_post_complete() returns data as an ARRAY, not as an OBJECT!!!
	print $pc['post_title'];  
	print $pc['my_custom_field'];
	print_r($pc); // <-- prints out all available fields, both built-in and custom fields
}
```