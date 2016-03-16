

# Description #

The _gallery_ filter formats one or many images using a formatting template.  This was developed specifically to help streamline the process of displaying multiple images. Available 0.9.5.6.

# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$input_ `[`, mixed _$options_ `]` )

  * **$input**: _string_ field name `+ :gallery`. Referenced field should contain an integer or an array of integers (post IDs).
  * **$options**: _string_: a formatting template containing placeholders for image data.  The default formatting string is the following:
```
<div class="cctm_gallery" id="cctm_gallery_[+i+]">
    <img height="[+height+]" width="[+width+]" 
        src="[+guid+]" title="[+post_title+]" alt="[+alt+]" 
        class="cctm_image" id="cctm_image_[+i+]"/>
</div>
```

Note: The formatting template includes several helpful placeholders designed specifically to assist the formatting of images.  These include the following:

  * `[+i+]` - a simple iterator, starting with 1 and counting up for each image that is formatted.
  * `[+alt+]` - contains the alternate text for an image.
  * `[+height+]` - the image height is calculated automatically for you.
  * `[+width+]` - the image width is calculated automatically for you.
  * `[+help+]` - as with other CCTM formatting templates, the _help_ placeholder can be used to list all available placeholders.

OUTPUT**: _string_.  An instance of the formatting template, processed once for each image.**

## Example 1 ##

The simplest invocation relies on the
```
<?php print_custom_field('my_images:gallery'); ?>
```


## Example 2: Supply your custom format ##

Instead of relying on the default formatting string, you can specify your own, e.g. to add your own "rel" attribute:

```
<?php print_custom_field('my_images:gallery', '<img height="[+height+]" width="[+width+]" src="[+guid+]" title="[+post_title+]" alt="[+alt+]" class="cctm_image" id="cctm_image_[+i+]" rel="something"/>'); ?>
```



# See Also #