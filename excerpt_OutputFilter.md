

# Description #

The _excerpt_ takes a long string and returns a shorter excerpt of it, either chopping the input on the separator string or limiting it to a given number of words depending on whether you supply a string or an integer as the optional argument.  The default behavior is to split on the `<!--more-->` separator.

# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$fieldname_ `[`, mixed _$options_ `]` )

  * **$fieldname**: _string_ field name `+ :excerpt`
  * **$options**: _none_.
  * **OUTPUT**: _string_.  An encoded version of the $input.

## Example 1 ##

This will split on `<!--more-->` (the default string separator used by WordPress)

```
<?php print_custom_field('my_long_textarea:excerpt'); ?>
```


## Example 2 ##

This will split on `===` (a custom string separator)

```
<?php print_custom_field('my_long_textarea:excerpt', '==='); ?>
```



## Example 3 ##

This will limit the input to 100 words.

```
<?php print_custom_field('my_long_textarea:excerpt', 100); ?>
```