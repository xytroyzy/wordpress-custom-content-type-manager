# get\_incoming\_links #

_array_ **get\_incoming\_links**()

If you have relation fields that have linked to a page, this function will show you which pages have linked there.  An array of post IDs is returned.

The function first gets the post ID for the post currently being viewed, e.g. `123`, then it finds a list of custom relation fields and finds any rows in the `wp_postdata` table which store data for those relation fields.  Any relation field storing a value of `123` is noted, and its parent post ID is returned.

The trick here is that the underlying database queries look for both the "raw" post ID (as stored by simple, non-repeatable fields) and for JSON-encoded post IDs (as stored by the repeatable fields).

# Examples #

## Example 1 ##

In your theme file, on a single-xxxxx.php file used to format a custom post-type.  As is the case throughout this plugin (and in any case where you store foreign key references), the name of the game is converting an integer post ID back into that post's data.

```
<?php
$links = get_incoming_links();
foreach($links as $l) {
?>
	<a href="<?php echo get_permalink($l); ?>">My link to a post or page</a>
<?php
}
?>
```

## Example 2 ##

A more useful example would involve retrieving more data from the other posts.  The [get\_post Output Filter](get_post_OutputFilter.md) is built for this very situation.

```
<?php
$links = get_incoming_links();
$posts = CCTM::filter($links, 'get_post');
foreach ($posts as $p) {
	printf('<a href="%s">%s</a>',get_permalink($p['ID']), $p['post_title']);
}
?>
```


# See Also #

  * [print\_incoming\_links](print_incoming_links.md)