This page contains examples of how to call and execute the [get\_posts](get_posts.md) function.  There are several variations.



# Calling the Function via PHP #

## In a Theme File ##

The usage in a theme file is very similar to WordPress' [WP\_Query](http://codex.wordpress.org/Function_Reference/WP_Query) object.  You can pass arguments to the constructor when you create it, you can pass arguments directly to the get\_posts function, or you can set the attributes on the object:

```

$Q = new GetPostsQuery();
$args = array();
$args['post_type'] = 'page';
$args['orderby'] = 'post_title';
$results = $Q->get_posts($args);

// Is equivalent to:

$args = array();
$args['post_type'] = 'page';
$args['orderby'] = 'post_title';
$Q = new GetPostsQuery($args);
$results = $Q->get_posts();

// Is equivalent to:
$Q = new GetPostsQuery();
$Q->post_type = 'page';
$Q->orderby = 'post_title';
$results = $Q->get_posts();

```

## Static Invocation ##

If you're intimidated by object oriented programming and you instead want a simpler replacement for the built-in **get\_posts()** function, use the static function:

```
$args = array();
$args['post_type'] = 'my_custom_post_type';
$args['date_min'] = '2011-01-31';
$results = SummarizePosts::get_posts($args);
```

If you use the static function, you must pass arguments directly to the get\_posts() function; you cannot set attributes on the **GetPostsQuery** object because by definition, if you are using a static function then you have not instantiated an object.


# Calling the Function via ShortCode #
This function can be accessed via [shortcode](http://codex.wordpress.org/Shortcode_API) in your WordPress posts. Note that WordPress shortcodes are only parsed when they appear in a post's main content block (the one with the WYSIWYG editor).

## Simplest Invocation ##

You can place the simple **summarize\_posts** shortcode into your content:
```
[summarize_posts]
```

The default values for all arguments will be assumed, and you will get a formatted unordered list of all your posts, e.g.

```
<ul class="summarize-posts">
	<li><a href="http://yoursite.com/some-page/">Man</li>
	<li><a href="http://yoursite.com/other-page/">Bear</li>
	<li><a href="http://yoursite.com/yet-another/">Pig</li>
</ul>
```

## Specifying Shortcode Arguments ##

Any argument from [get\_posts()](get_posts.md) can be used in a shortcode invocation, e.g.:

```
[summarize_posts post_type="teaser" parent="149" order="ASC"]
```

The above causes only "teaser" post types (an example custom content type) which are children of post #149 to be listed in ascending order.

The above shortcode is equivalent to the following in a theme file:
```
$Q = new GetPostsQuery();
$Q->post_type = 'teaser';
$Q->parent = '149';
$Q->order = 'ASC';
$results = $Q->get_posts();
// ... iterate over the results... 
```

## Specifying Shortcode Format ##

What if you don't want the simplistic unordered list?

You can specify a few other arguments here to help you format your results.  Use the full version of the shortcode to specify the formatting template to be used to format each result.

```
[summarize_posts]<li>[+post_title+]</li>[/summarize_posts]
```

You can also use some additional properties to format the results:

  * **before**: printed before the results. default = `<ul class="summarize-posts">`
  * **after**: printed after the results. default =  `</ul>`,
  * **paginate**: whether or not to paginate the results. default = false
  * **tpl** you can specify a path (relative to webroot) to a file to be used as the template. For example, a value of "wp-content/my.tpl" would look inside the "wp-content" folder for a file named "my.tpl". <font color='red'>The file <b>must</b> use a <code>.tpl</code> extension!</font> default = `null`
  * **help** If set, the output will include debugging info and examples. default = `false`