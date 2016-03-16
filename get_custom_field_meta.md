# get\_custom\_field\_meta #

_mixed_ **get\_custom\_field\_meta**( string _$fieldname_ `[`, string _$item_`]` )

This allows you to get information about the custom field's definition (i.e. its meta data).  Just provide the unique name of the field, which bit of data you want, and its definition data will be available to you.

### $fieldname ###
(string) (required) The name of the custom field whose value you want to get.

### $item ###
(string) (optional) The name of the definition item for this custom field whose value you want to get.

## Example ##

If you supply both arguments, you can get exactly the data you want.

```
<?php 
// This might print "My Custom Field Label"
print get_custom_field_meta('my_custom_field','label'); 
?>
```

## Example 2 ##

If you omit the 2nd argument, the entire definition array is returned.

```
<?php
$metadata = get_custom_field_meta('my_dropdown');
 ?>
```

The output might be something like this:

```
Array
(
    [label] => My Dropdown
    [name] => my_dropdown
    [description] => Just an example desc
    [type] => dropdown
    [options] => Array
        (
            [0] => Male
            [1] => Female
        )

    [default_value] => Female
    [sort_param] => 2
)
```