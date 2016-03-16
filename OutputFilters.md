The reason the CCTM uses Output Filters is pretty much the same reason WordPress uses its own [filters](http://codex.wordpress.org/Plugin_API): they are used to modify data before sending it to the browser screen.

This is particularly confusing for some users when it comes to [Relation](Relation.md), [Image](Image.md), and [Media](Media.md) fields because the only thing that's stored in those relation fields is the _post ID_ of the related post, image, or media item you selected (it's a [foreign key](http://en.wikipedia.org/wiki/Foreign_key)).  Most of the time, you don't want the number itself, you want the post info that is _represented_ by that number.  The solution is to use an Output Filter to convert the number into an img tag or to the src of the image.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=8VKvwHncOEE' target='_blank'><img src='http://img.youtube.com/vi/8VKvwHncOEE/0.jpg' width='425' height=344 /></a>

## Default Output Filter ##

Every custom field can specify a Default Output Filter.  This filter will be used on any standard instance of `get_custom_field()` or `print_custom_field()`.

If you have a custom field name **my\_field** and you have defined it to use a default output filter of [to\_image\_src](to_image_src_OutputFilter.md), then the following two calls to `get_custom_field()` are equivalent:

```
<?php
get_custom_field('my_field');  // <-- uses the default output filter
get_custom_field('my_field:to_image_src'); // <-- verbosely specifies the output filter
?>
```

**REMEMBER:** specifying an output filter by name will **_override_** any default output filter set.

## Chaining ##

You can chain several Output Filters together by appending their names after a colon:

```
<?php
get_custom_field('my_field:filter1:filter2:etc');
?>
```

You can supply options to each filter by suppling additional arguments to the `get_custom_field()` or `print_custom_field()`functions:

```
<?php
get_custom_field('my_field:filter1:filter2', 'option 1', 'option 2');
?>
```

The first option argument is passed to the first filter (or to the default filter); the second option argument is passed to the second filter, and so on:

![https://img.skitch.com/20110926-b7nqtrgeqn7pfcgckm6qcuwghd.png](https://img.skitch.com/20110926-b7nqtrgeqn7pfcgckm6qcuwghd.png)

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | It is not recommended to chain together more than 2 output filters because your code becomes very difficult to read!   |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------|

## Standalone Use ##

Although the primary use of Output Filters is when they are used directly to modify the output of a custom field, the functions can also be called on their own: they are functions that modify an input.  Use the `CCTM::filter()` function to filter input through any of the available output filters.

```
<?php
$filtered_value = CCTM::filter( $value, $filtername [, $options]);
?>
```

  * **$value** : the value that you want to filter
  * **$filtername** : any valid output filter
  * **$options** : some filters use options to affect their output.  See the documentation for each filter.


### Example ###

```
<?php
$my_thumb = CCTM::filter( 123, 'to_image_tag', 'thumbnail') );
?>
```

In this example, we assume that 123 is the post ID of an attachment post (i.e. an image post).


| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | Using the standalone **CCTM::filter()** function does not allow for automatic chaining: you would have to feed the output of one instance of this function into the input of another. |
|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## See Also ##

The formatting strings used by the various Output Filters rely in the CCMT's [Parser](Parser.md).  This can trigger some useful functionality.