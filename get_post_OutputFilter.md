

# Description #

The _get\_post_ filter returns an associative array of entire post including all custom fields when given a post ID.

# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$input_ `[`, mixed _$options_ `]` )

  * **$input**: _string_ field name `+ :get_post`.  Referenced field should contain an integer or an array of _integers_ (post IDs).
  * **$options**: _string_: either supply the name of a field (e.g. **post\_title**), OR supply a formatting string, e.g. `My post title is [+post_title+]`.
  * **OUTPUT**: _mixed_.  If no options are supplied, this returns one or more associative arrays representing the post and all of its custom fields.  If you have supplied an option, this will return a string.

## Example 1 ##

```
<?php 

$post = get_custom_field('my_relation:get_post'); 
print $post['post_title'];
print $post['my_custom_field'];
?>
```

## Example 2 ##

You can use this on a "repeatable" multi-relation field to get custom-field and other data from the related posts.

```
<?php 

$posts_array = get_custom_field('my_relation:to_array', 'get_post'); 
foreach ($posts_array as $p) {
   printf('<a href="%s">%s</a>: %s', $p['guid'], $p['post_title'], $p['my_custom_field']);
}
?>
```

## Example 3 ##

A more explicit variation on Example 2 above, used to get full post data from the related posts.  Note here the use of WordPress' [get\_permalink()](http://codex.wordpress.org/Function_Reference/get_permalink) function to convert the post ID into the URL of the post (using the post's guid should have the exact same effect):

```
<?php 

$posts_array = get_custom_field('my_relation:to_array'); 
foreach ($posts_array as $post_id) {
   $post = CCTM::filter($post_id, 'get_post');
   printf('<a href="%s">%s</a>: %s', get_permalink($post['ID']), $post['post_title'], $post['my_custom_field']);
}
?>
```

## Example 4 ##

You can use the get\_post filter standalone too -- this is useful if you need to convert a post ID into that post's attributes.

```
<?php
$my_post = CCTM::filter( 123, 'get_post');
print $my_post['guid'];
print $my_post['some_custom_field'];
// print_r($my_post); // <-- this will show you which fields are available
?>
```

## Example 5 ##

People often get confused when they need to retrieve a related image of a related post -- it's the same problem with foreign key associations, but it's one more level deep.

For example, if you want to print the featured image of a related post, you can use get\_post to get all the custom fields of the related post -- a reference to its featured image is stored as `thumbnail_id`.  Then you can use a built-in WordPress function or another CCTM filter to convert that id to an image.

```
<?php
// Use wp_get_attachment_image
$related_post = get_custom_field('my_relation:get_post'); 
$img = wp_get_attachment_image($related_post['thumbnail_id']);
print $img;

// Or use CCTM's to_image_src filter
$related_post = get_custom_field('my_relation:get_post'); 
$img_src = CCTM::filter($related_post['thumbnail_id'], 'to_image_src');
print '<img src="'.$img_src.'" />';

?>
```

This technique is possible with any image field.

# See Also #

  * [to\_image\_array](to_image_array_OutputFilter.md) Output Filter
  * [to\_image\_tag](to_image_tag_OutputFilter.md) Output Filter