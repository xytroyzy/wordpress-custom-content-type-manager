This page contains examples of how to use the GetPostsQuery class' [get\_posts](get_posts.md) function.   This function offers a powerful and flexible interface into the WordPress database.  It allows you to retrieve posts and their related data via a unified API via efficient database queries, and it avoids the confusing and inconsistent WordPress functions.

If you're unclear on how to call the function, see [Calling get\_posts()](calling_get_posts.md).




---


# Examples #

This section is focused on example queries possible with get\_posts.

IN PROGRESS...

## Simplest Example ##

The simplest possible example here is to fetch posts with no special arguments -- here's an example for retrieving posts.

```
<?php
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'post';
$results = $Q->get_posts($args);

foreach ($results as $r) {
    print $r['post_name'] . '<br/>';
    print $r['post_type'] . '<br/>';
    print $r['ID'] . '<br/>'; 
}
?>
```

Note that the attributes available to the individual results ($r in this example) correlate to the names of the columns in the `wp_posts` table and to the names of your custom fields.  Use `print_r($r)` to dump the entire contents of any single item and see which attributes are available for you.

It's a bit sloppy to print out data like this because you must concatenate PHP variables and HTML strings; please use your own judgement as to which format is most appropriate, but a common approach is to switch back into HTML mode when printing your results and use the long-form foreach statement like this:

```
foreach ($results as $r):
?>
    <p><?php print $r['post_name'; ?></p>
<?php
endforeach;
```


## List Posts in a Category ##

In your template file, you can list all posts in a category by making use of the `taxonomy` and `taxonomy_slug` arguments.

```
<?php
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'article';
$args['post_status'] = 'publish';
$args['taxonomy'] = 'category';
$args['taxonomy_slug'] = 'howto';
$args['orderby'] = 'menu_order';
$args['order'] = 'ASC';

$results = $Q->get_posts($args);

foreach ($results as $r):
?>
	<a href="<?php print $r['permalink']; ?>" rel="bookmark"><?php print $r['post_title']; ?></a> <br />
<?php
endforeach;
?>
```

## List Posts and Their Categories ##

Looking up a post's categories requires use of built-in WordPress functions, e.g. [get\_the\_category()](http://codex.wordpress.org/Function_Reference/get_the_category) or similar -- you need to look up the categories associated with a post by its post ID.

```
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'your_post_type';
$results = $Q->get_posts($args);
foreach ($results as $r) {
    print '<h3>'.$r['post_title'].'</h3>';
    $categories = get_the_category($r['ID']); // <-- what categories belong to this post ID?
    print 'Categories this post is associated with: ';  // (this is an array of values we must loop over)
    foreach ($categories as $c) {
        print '<a href="'.get_category_link( $c->term_id ).'" title="' . esc_attr( sprintf( __( "View all posts in %s" ), $c->name ) ) . '">'.$c->cat_name.'</a>'
    }
}
?>
```

## List Posts Hierarchically by Category ##

<font color='red'>Warning: This is tough.</font>  The problem is that people usually place posts into BOTH the parent AND the child categories, so when you iterate through the posts in each category, the post will be listed in both lists (once for the list of all posts in the parent category, and again in the lists of all posts in the child category).

For example, if you have a parent category named "Mammals" and a child-category named "Dogs".  When you create a new "animal" post, most users will check _both_ the parent "Mammals" category and the child "Dogs" category (indeed, this is the direction the WordPress UI pushes you).  This can present a problem because technically speaking, the post now exists in _both_ categories, so it will show up in _both_ lists.  The problem is compounded by the fact that WordPress GUI does not behave gracefully when you don't place a post into the parent and the child categories.

Nevertheless, here is an example of how you might generate a list of posts by categories:

```
<?php
// Some optional filters are shown here on the initial list of categories
$categories = get_categories(array('child_of' => $some_category)); 

foreach ($categories as $c) {

	print '<h1>'. $c->name . '</h1>';

	// Parent Categories (optionally omit this) --------
	$Q = new GetPostsQuery();
	$args = array();
	$args['post_type'] = 'article';
	$args['taxonomy'] = 'category';
	$args['taxonomy_depth'] = 1;
	$args['taxonomy_slug'] = $c->slug;

	$results = $Q->get_posts($args);

	// These are any posts that are in the parent category
	foreach ($results as $r) {
		printf('<a href="%s">%s</a><br/>', $r['permalink'], $r['post_title']);
	}
	//----- (omit to here) ------------------------

	$subcategories = get_categories(array('child_of' => $c->slug)) {

	foreach ($subcategories as $s) {	
		
		// Sub Categories
		print '<h2>'. $s->name . '</h2>';
		
		$Q = new GetPostsQuery();
		$args = array();
		$args['post_type'] = 'article';
		$args['taxonomy'] = 'category';
		$args['taxonomy_depth'] = 1;
		$args['taxonomy_slug'] = $s->slug;
	
	
		$results = $Q->get_posts($args);
		
		foreach ($results as $r) {
			printf('<a href="%s">%s</a><br/>', $r['permalink'], $r['post_title']);
		}
	}
}
?>
```

In the above example, you may choose to omit the query that selects posts in the parent category.  If you omit that section, your results may become cleaner, but some posts may disappear off the radar.  For example, if you have a post that is _only_ in the "Mammals" parent category and you aren't printing those posts, the post will not be visible.

Remember: this problem occurs at every level in the hierarchy!


## List Posts in a Date Range ##

A date range with just one parameter is easier than a from/to date range.  If you just need posts that are before or after a certain date, just use an operator in your arguments, e.g. the ">" greater than operator:

```
<?php
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'page';
$args['post_date']['>'] = '2005-01-30';
$args['order'] = 'ASC';

$results = $Q->get_posts($args);

foreach ($results as $r):
    // print stuff here
endforeach;
?>
```

For using a date range, you should refer to the [get\_posts](get_posts.md) documentation, specifically the **date\_min** and **date\_max** and **date\_column** arguments.

```
<?php
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'page';
$args['date_column'] = 'post_date';
$args['date_min'] = '2007-03-01';
$args['date_max'] = '2007-03-31';
$args['order'] = 'ASC';

$results = $Q->get_posts($args);

foreach ($results as $r):
    // print stuff here
endforeach;
?>
```

You can also use the **date\_min** and **date\_max** to filter on a custom date field, <font color='red'><b>but you must store your dates in the MySQL format!</b></font>  This is critical: you can use the [datef\_OutputFilter](datef_OutputFilter.md) to display dates to your visitors however you want, but it is strongly recommended that you always store your date values in a sortable format.

```
<?php
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'movie';
$args['date_column'] = 'release_date';
$args['date_min'] = '2007-03-01';
$args['date_max'] = '2007-03-31';
$args['order'] = 'ASC';

$results = $Q->get_posts($args);

foreach ($results as $r):
    // print stuff here
endforeach;
?>
```


A final example here uses the _starts\_with_ operator.  So long as you have stored your dates using the correct !MySQL format of YYYY-MM-DD, then something like the following can retrieve all the posts, say, in a given year:

```
<?php
$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'my_post_type';
$args['my_date']['starts_with'] = '2007-';
$results = $Q->get_posts($args);

foreach ($results as $r):
    // print stuff here
endforeach;
?>
```

The downside to using operators like this is that they are not available to shortcodes: you must code them in PHP.


---


## Searching For a Search Term (1) ##

Here's the standard example of searching both the title and the content fields for a search term:

```
$Q = new GetPostsQuery();
$args = array();
$args['search_columns'] =  array('post_title', 'post_content');
$args['search_term'] = 'wordpress';
$results = $Q->get_posts($args);

foreach ($results as $r) {
    // print stuff here
}
```

## Searching For a Search Term (2) ##

Here's a variation of the first search, where we want to match only records that _start_ with the search term.  We control the behavior using the **match\_rule** argument.  This is a bit simpler than using the operators, but it has the advantage that it can be performed via a shortcode.

```
$Q = new GetPostsQuery();
$args = array();
$args['search_columns'] =  array('post_title');
$args['search_term'] = 'wordpress';
$args['match_rule'] = 'starts_with';
$results = $Q->get_posts($args);

foreach ($results as $r) {
    // print stuff here
}
```




---


# Troubleshooting #

## get\_posts returns the completely wrong post ##

Be careful that you don't accidentally use `get_post` when you meant to use `get_posts`.  `get_post` expects an integer post ID as input, so if you instead pass it a series of arguments intended for `get_posts`, this will evaluate to some integer via some incomprensible bit of PHP number crunching, and you may get a result from the database that is completely unexpected.