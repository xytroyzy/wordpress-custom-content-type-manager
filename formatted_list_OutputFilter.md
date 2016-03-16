

# Description #

The **formatted\_list** filter takes an array and formats it formats it into a sensible string for your theme files.  It offers a quick way to generate a formatted list without you having to loop over array items.

# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$fieldname_ `[`, mixed _$options_ `]` )

  * **$fieldname**: _string_ field name `+ :formatted_list`. Your fields should contain an array of values.
  * **$options**: _mixed_. If a string is supplied, it will be used to join all array elements together. You can also supply an array of formatting strings where the first string in the array will format each item in the list, and the second string in the array will wrap the output.  See the examples below.
  * **OUTPUT**: _string_.

## Example 1 ##

Use a simple string to join together the array elements (just like PHP's [implode](http://php.net/manual/en/function.implode.php) function):

```
<?php
// my_list contains a JSON array, e.g. ["man","bear","pig"]
print_custom_field('my_list:formatted_list', ', ');
// prints a comma-separated list: man, bear, pig
?>
```

Note that it is also possible to use an associative array as the input.  If you use a simple string separator to operate on an associative array, only the array _values_ will be joined.

## Example 2 ##

For more control over the output, you can pass an array to the **$options** parameter.  The array must be in the following format:

array( _string_ **item\_template** `[`, _string_ **wrapper\_template** `]`)

The **item\_template** and **wrapper\_template** are expected to be valid formatting strings that use `[+placeholders+]`.

**item\_template**: used to format each item in the list.  Available placeholders:

  * **`[+key+]`** : will be replaced with the key of the item
  * **`[+value+]`** : will be replaced with the value of the item

Note that the `[+key+]` will be an integer unless the inputted $array was an associative array.

**wrapper\_template**: (optional) used to wrap the final output.  Available placeholders:

  * **`[+content+]`**: will be replaced with all formatted items


```
print_custom_field('my_list:formatted_list', array('<li>[+value+]</li>','<ul>[+content+]</ul>') );
/* prints a formated unordered list:
<ul>
  <li>man</li>
  <li>bear</li>
  <li>pig</li>
</ul>
*/
```


## Example 3 ##

How about a list of posts selected via a [Relation](Relation.md) field?  Remember: relations are just foreign post IDs, so you are storing an array of numbers.  The task is to convert the numbers into something useful.  We can leverage the secondary filters in the CCTM's [Parser](Parser.md) by using the expanded syntax.

```
<?php
print_custom_field('my_posts:formatted_list', 
	array('[+value:get_post==<a href="{{permalink}}">{{post_title}}</a>+]<br/>',
	'<strong>My Posts</strong><br/>[+content+]') );
?>
```

Tricky, eh?  The `[+value+]` placeholder holds a simple integer page id, so then we use the [get\_post](get_post_OutputFilter.md) Output Filter to convert to a link.  We have to supply the `get_post` filter with options, and we do that via an extended syntax.  The output is something like this:

```
<strong>My Posts</strong><br/>
<a href="mysite.com/link/to/post1">First Post</a><br/>
<a href="mysite.com/link/to/post2">Second Post</a><br/>
```

### Variation 1 ###

You could get similar results using the [to\_array](to_array_OutputFilter.md) Output Filter and passing it as an argument the [to\_link](to_link_OutputFilter.md) filter:

```
$list = get_custom_field('my_posts:to_array', 'to_link');
print implode('<br/>', $list);
```

However, that option is not as flexible because you can't pass options to the secondary `to_link` filter.

### Variation 2 ###

You could also use the [get\_post](get_post_OutputFilter.md) Output Filter to operate on the array of post ids:

```
print_custom_field('my_posts:get_posts', '<a href="[+permalink+]">[+post_title+]</a><br/>');
```

Or even more flexible, you could chain this with the [wrapper](wrapper_OutputFilter.md) Output Filter:

```
print_custom_field('my_posts:get_posts:wrapper'
    , '<a href="[+permalink+]">[+post_title+]</a><br/>'
    , '<strong>My Posts</strong><br/>[+content+]');
```



# See Also #

  * [to\_array](to_array_OutputFilter.md) Output Filter