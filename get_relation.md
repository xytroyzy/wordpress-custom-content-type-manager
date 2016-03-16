# get\_relation #

_array_ **get\_relation**( string _$fieldname_ )

This is similar to WordPress' get\_post() function, but instead we lookup a related post by the fieldname that contains the reference to the post and not by the post ID.  A relation field stores a post ID, and that ID identifies another post.  So given
a fieldname, this returns the complete post array for that was referenced by
the custom field.

### $fieldname ###
(string) (required) The name of the custom field.

## Example ##

```
<?php
$my_related_post = get_relation('my_rel');
print $my_related_post['post_content'];
?>
```

The equivalent "long-form" of this might be something like this:

```
$rel_post_id = get_custom_field('my_rel:raw'); // get the post ID

$post = get_post_complete($rel_post_id);  // or get_post()
```