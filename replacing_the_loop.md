

# Overview #

"The Loop" is one of those WordPress internals that every user hears about eventually.  It can be murder to customize it because the architecture is poorly conceived: The Loop relies on myriad global variables and array pointers, so customizing certain aspects of it is nearly impossible.  WordPress' query\_posts() function only goes so far in allowing sensible overrides, and often times it makes the problem worse.

I have often had to replace the loop on certain pages in order to achieve the customization I required.  Some of the cases where I had to abandon the loop included the following:

  * Limiting the number of posts differently between different pages.  E.g. category X shows 5 posts per page, but category Y shows 7 posts per page.
  * Complex sorting.  E.g. sorting a set of results by a custom column, or doing complex sorting on multiple columns.
  * Customizing search fields (e.g. filtering on custom fields)
  * Customized pagination links.


# Equivalent Functions #

If you need to replace the WordPress loop with your own home-rolled version, you realize that there are a lot of functions that you need to use.  It can quickly become exasperating to realize how many WordPress functions _print_ data instead of _returning_ it, and worse yet, many of them do not accept input, instead they rely on global variables set up by the various looping functions.  It's an architectural nightmare hidden away in a template file.

Fear not: there are solutions.  For most functions in the loop, there are equivalents that can work in your own home-rolled loop.

For the following examples, assume your theme file includes a GetPostsQuery query something like the following:

```
require_once(CCTM_PATH.'/includes/GetPostsQuery.php');
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'person';
$args['show_on_archive'] = 1;
$args['orderby'] = 'last_name';
$args['order'] = 'ASC';
$args['limit'] = 10; // 
$args['paginate'] = true;

if (isset($_GET['offset'])) {
	$args['offset'] = (int) $_GET['offset'];
}

$results = $Q->get_posts($args);

print $Q->get_pagination_links();

foreach ($results as $r):  // Begin our home-rolled loop:
?>


<?php

endforeach;
?>
```

## `$wp_query->max_num_pages` ##

Typically, WordPress uses this to determine whether or not you need to show pagination links, e.g.

```
<?php if ( $wp_query->max_num_pages > 1 ) : ?>
    <div class="nav-previous"><?php next_posts_link( __( '<span class="meta-nav">&larr;</span> Older posts', 'twentyten' ) ); ?></div>
<?php endif; ?>
```

However, when using GetPostsQuery, this is unnecessary.  GetPostsQuery will automatically detect whether pagination links are required, so you can simply do the following:

```
<?php print $Q->get_pagination_links(); ?>
```


## have\_posts() ##

Used to trigger a "sorry" message, e.g.

```
<?php if ( ! have_posts() ) : ?>
	<p><?php _e( 'Apologies, but no results were found for the requested archive. Perhaps searching will help find a related post.', 'twentyten' ); ?></p>
<?php endif; ?>
```

When using GetPostsQuery, you can simply test your $results to see if it is empty or not:

```
<?php if (empty($results)) : ?>
	<p><?php _e( 'Apologies, but no results were found for the requested archive. Perhaps searching will help find a related post.', 'twentyten' ); ?></p>
<?php endif; ?>
```

## have\_posts() and the\_post() ##

These functions are the foundation of the built-in WordPress loop, e.g.

```
<?php while ( have_posts() ) : the_post(); ?>
    ... stuff here...
<?php endwhile; ?>
```

When using GetPostsQuery, you simply iterate over your result set:

```
<?php foreach ($results as $r): ?>
    ... stuff here...
<?php endforeach; ?>
```

## the\_ID() ##

This prints out the id of the current post in the loop.  Since it relies on global variables, it's not suitable for use anywhere else (i.e. in a _normal_ loop).

When using GetPostsQuery, the post id is available like any other post attribute.  Simply print it:

```
<?php print $r['ID']; ?>
```

## post\_class() ##

This is used to come up with a CSS class for each post, e.g.

```
<div id="post-<?php the_ID(); ?>" <?php post_class(); ?>>
```

When using GetPostsQuery, you have to pass the function a couple extra arguments:

```
<div id="post-<?php print $r['ID']; ?>" <?php post_class('', $r['ID']); ?>>
```

## the\_permalink() ##

This prints out the permalink for the active post.  When using GetPostsQuery, "permalink" is an attribute available like any other attribute:

```
<a href="<?php print $r['permalink']; ?>"><?php print $r['post_title']; ?></a>
```

## the\_title\_attribute() ##

This is usually used to print the post's title inside an attribute (usually inside a link tag):

```
<a href="<?php the_permalink(); ?>" title="<?php printf( esc_attr__( 'Permalink to %s', 'twentyten' ), the_title_attribute( 'echo=0' ) ); ?>" rel="bookmark"><?php the_title(); ?></a>
```

When using GetPostsQuery, you can print the post's title, just filter out any inappropriate contents that may be there:

```
<a href="<?php print $r['permalink']; ?>" title="<?php print htmlspecialchars($r['post_title']; ?>" rel="bookmark"><?php print $r['post_title']; ?></a>
```

## the\_title() ##

Used to print the post's title.  When using GetPostsQuery, you can print the post's title like any other attribute:

```
<?php print $r['post_title']; ?>
```

## has\_post\_thumbnail() ##

Tests whether the current post has a thumbnail (aka featured image) associated with it.  When using GetPostsQuery, you can test whether the result has an empty value for `thumbnail_id`:

```
if (!empty($r['thumbnail_id'])) {
   // ... do something with the thumbnail
}
```

## the\_post\_thumbnail() ##

This function will print a thumbnail image (with some variations possible).  You can exchange it for `get_the_post_thumbnail()` and pass it extra arguments as needed:

```
<?php
print get_the_post_thumbnail( $r['ID'], 'person-thumb', array('class' => 'alignnone size-full')); 
?>
```

## get\_the\_category() ##

This function optionally can be supplied a post ID, so we need to do that when iterating over results:

```
get_the_category($r['ID']));
```

## get\_the\_category\_list() ##

This generates a list of categories that each post has been placed in, e.g.

```
<?php printf( __( '<span class="%1$s">Posted in</span> %2$s', 'twentyten' ), 'entry-utility-prep entry-utility-prep-cat-links', get_the_category_list( ', ' ) ); ?>
```

When using GetPostsQuery, you would need to supply it with some extra parameters:

```
<?php printf( __( '<span class="%1$s">Posted in</span> %2$s', 'twentyten' ), 'entry-utility-prep entry-utility-prep-cat-links', get_the_category_list( ', ','',$r['ID'] ) ); ?>
```

## get\_the\_tag\_list() ##

This generates a list of tags that each post has been tagged with:

```
<?php
	$tags_list = get_the_tag_list( '', ', ' );
	if ( $tags_list ):
?>
	<span class="tag-links">
		<?php printf( __( '<span class="%1$s">Tagged</span> %2$s', 'twentyten' ), 'entry-utility-prep entry-utility-prep-tag-links', $tags_list ); ?>

	</span>
<?php endif; ?>
```

When using GetPostsQuery, we have to do some workarounds because there isn't a drop-in replacement function that can get the tags for any post.

```
<?php
	$tags_list = get_the_tags($r['ID']);
	if ( $tags_list ):
		$tags_array = array();
		foreach ($tags_list as $t) {
			$tags_array[] = sprintf('<a href="%s">%s</a>', get_tag_link($t->term_id), $t->name);
		}
?>
	<span class="tag-links">
		<?php printf( __( '<span class="%1$s">Tagged</span> %2$s', 'twentyten' ), 'entry-utility-prep entry-utility-prep-tag-links', implode(', ',$tags_array) ); ?>
	</span>
<?php endif; ?>
```