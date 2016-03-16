This function is behind all the power of the CCTM's [Output Filters](OutputFilters.md).  Normally, you use it indirectly via the [get\_custom\_field](get_custom_field.md) or [print\_custom\_field](print_custom_field.md) functions, but you can call this function directly.

# CCTM::filter #

_mixed_ **CCTM::filter**( mixed _$value_, string _$output\_filter_, `[`, mixed _$options_ `]` )


### $value ###
(mixed) (required) The value you want to filter.

### $output\_filter ###
(string) (required) The name of any available [Output Filter](OutputFilters.md).

### $options ###
(mixed) (optional) Any options to be passed to the output filter


# Examples #


## Example 1: Fetching an Image src ##

Say for example that post ID 456 is an image that you uploaded to your site via the Media Uploader.  The following code in your template would retrieve the image's src:

```
<img src="<?php print CCTM::filter(456, 'to_image_src'); ?>" />
```