

# Description #

The **to\_link\_href** converts a post ID to the full link to that post (the link's [href](http://www.w3schools.com/tags/att_a_href.asp) attribute).  <font color='red'>Do not use this filter on URLs! It operates on integers only!</font>


# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$input_ `[`, mixed _$options_ `]` )

  * **$input**: _string_.  an integer post ID
  * **$options**: _string_. Default value to return if no post ID is specified.
  * **OUTPUT**: _string_. The URL for the referenced post.

# Options #

Supply a default value if there is no post ID supplied.  You can use this to supply a default href.

# Examples #

## Example 1 ##

Gets the full URL to the referenced post.

```
<a href="<?php print_custom_field('my_relation:to_link_href'); ?>">Click here!</a>
```

## Example 2 ##

You can use any output filter in the [CCTM::filter()](CCTM_filter.md) function.  This should show clearly that this filter operates _only_ on integers.  In this example, 345 is the post ID of the post we're linking to:

```
<a href="<?php print CCTM::filter(345, 'to_link_href'); ?>">Click here!</a>
```


## Example 3 (missing posts) ##

If you are trying to link to a post that has been deleted, an error message will be returned.  This might also occur if you have migrated sites and your custom field is pointing to a post that does not exist in the new database.

**Error Message:** <font color='red'>Referenced post not found.</font>

```
// E.g. if my_relation contains post ID 123, and post ID 123 has been deleted
<a href="<?php print_custom_field('my_relation:to_link_href'); ?>">Click Here</a>
// Prints out "Referenced post not found."
```

## Example 4 (invalid input) ##

Just to be abundantly clear: this filter is meant to operate on _integer post IDs_.  If your field does not contain a post ID, then do not use this filter.  <font color='red'>Do not pass it full URLs!</font>

**Error Message:** <font color='red'>Invalid input. to_link_href operates on post IDs only.</font>

```
// E.g. if my_text contains a URL like http://google.com/ then the function will choke:
print_custom_field('my_text:to_link'); 
// Prints out "Invalid input. to_link_href operates on post IDs only."
```

Don't despair: it's a trivial task to print out your text field and treat it like a URL.  It's just basic HTML, e.g.

```
<a href="<?php print_custom_field('my_url:raw'); ?>">Click Here!</a>
```

# See Also #

[to\_link](to_link_OutputFilter.md) Output Filter