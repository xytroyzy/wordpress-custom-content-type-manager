# print\_custom\_field\_meta #

This is a convenience function that simple prints the output of the [get\_custom\_field\_meta()](TemplateFunctions#get_custom_field_meta.md) function.

_mixed_ **print\_custom\_field\_meta**( string _$fieldname_, string _$item_ )

This allows you to print information about the custom field's definition (i.e. its meta data).  Just provide the unique name of the field, which bit of data you want, and its definition data will be available to you.

### $fieldname ###
(string) (required) The name of the custom field whose value you want to get.

### $item ###
(string) (required) The name of the definition item for this custom field whose value you want to get.

## Example ##

Examples are included in this Plugin's administration page: just click the link for "Show Sample Templates".

```
<?php 
// This might print "My Custom Field Label"
print_custom_field_meta('my_custom_field','label'); 
?>
```