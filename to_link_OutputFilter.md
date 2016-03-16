

# Description #

The **to\_link** converts a post ID to a full anchor tag (a link).  <font color='red'>Do not use this filter on URLs! It operates on integers only!</font>


# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$input_ `[`, mixed _$options_ `]` )

  * **$input**: _string_.  an integer post ID
  * **$options**: _string_. (optional) The "click me" text. The default value is the post's title. OR, you may also supply this argument a full formatting string with placeholders (similar to the [get\_post](get_post_OutputFilter.md) output filter).
  * **OUTPUT**: _string_. The URL for the referenced post.

# Options #

You can optionally specify the "click me" text for the link.


# Examples #

## Example 1 ##

Gets a full anchor tag to the referenced post.

```
<?php
print_custom_field('my_relation:to_link','Read more...'); 
// Prints out something like:
// <a href="http://yoursite.com/some/post/">Read more...</a>
?>
```

## Example 2 ##

Just to demonstrate that this filter operates solely on integers, here's an example of how you could call this filter on a raw value using the [CCTM::filter()](CCTM_filter.md) function.  Assume that you want to print a link to post ID 42:

```
<?php
print CCTM::filter(42,'to_link');
// prints out something like <a href="http://yoursite.com/your-post/" title="Your Post">Your Post</a>
?>
```

## Example 3 (missing posts) ##

If you are trying to link to a post that has been deleted, an error message will be returned.  This might also occur if you have migrated sites and your custom field is pointing to a post that does not exist in the new database.

**Error Message:** <font color='red'>Referenced post not found.</font>

```
// E.g. if my_relation contains post ID 123, and post ID 123 has been deleted
print_custom_field('my_relation:to_link'); 
// Prints out "Referenced post not found."
```

## Example 4 (invalid input) ##

Just to be abundantly clear: this filter is meant to operate on _integer post IDs_.  If your field does not contain a post ID, then do not use this filter.  <font color='red'>Do not pass it full URLs!</font>

**Error Message:** <font color='red'>Invalid input. to_link operates on post IDs only.</font>

```
// E.g. if my_text contains a URL like http://google.com/ then the function will choke:
print_custom_field('my_text:to_link'); 
// Prints out "Invalid input. to_link operates on post IDs only."
```

Don't despair: it's a trivial task to print out your text field and treat it like a URL.  It's just basic HTML, e.g.

```
<a href="<?php print_custom_field('my_url:raw'); ?>">Click Here!</a>
```

## Example 5 (formatting string) ##

You can also supply a full formatting string to this filter for customizing the output.

```
print_custom_field('my_relation:to_link', '<href="[+permalink+]" class="my_class">Read More: [+post_title+]</a>'); 
// prints out custom link format
```


# See Also #

[to\_link\_href](to_link_href_OutputFilter.md) Output Filter