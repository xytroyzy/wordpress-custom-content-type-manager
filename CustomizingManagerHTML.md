With version 0.9.4, you can customize the HTML that appears in the WordPress manager when displaying your custom fields.  The system here relies on formatting strings, similar in concept to those used by PHP's [sprintf](http://php.net/manual/en/function.sprintf.php) function, but instead of using nondescript `%s` instances to denote where values will be placed, we instead use `[+placeholders+]` denoted with square brackets and plus signs.  These `[+placeholders+]` are then parsed by the CCTM [Parser](Parser.md).



# Overview #

Most custom fields use two formatting templates to generate the HTML that you see in the WordPress manager when you edit or create a post.  The most common formatting templates are as follows:

  * **field** : the field tpl file formats the value and information
  * **wrapper** : the wrapper tpl wraps the field tpl (or field tpls)

Most often, the wrapper wraps only one instance of the field, but if your field is "repeatable" and can store multiple selections, then the wrapper will wrap multiple instances of the field.

![http://s7.postimage.org/vg0t7txx7/custom_field_wrapping.jpg](http://s7.postimage.org/vg0t7txx7/custom_field_wrapping.jpg)


# File Locations #

To begin understanding this, have a look at the `custom-content-type-manager/tpls` directory ("tpls" here is short for "templates").  There is a series of sub-directories there, each one dedicated to formatting a different part of the CCTM's output.  See [FolderStructure](FolderStructure.md) for more information.

Note that filenames beginning with an underscore, e.g. `_image.tpl` are intended as a _special_ formatting template, used to format a _type_ of field.


| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | <font color='red'>WARNING</font>: do not edit the files in the `custom-content-type-manager/tpls` directory.  Instead, copy the directory (or parts of it) to your `wp-content/uploads/cctm` directory.  (If you've customized the location of your uploads directory, then move the `tpls` directory into your custom location).|
|:--------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

# Hierarchy #

The basic idea of this file structure is that users can override the built-in templates contained in the `plugins` directory with their own files created in the `uploads` directory.  The user-created directory inside of `wp-content/uploads/cctm/tpls` is searched before the built-in directory inside of `custom-content-type-manager/tpls`; that way templates can be easily overridden by the user.

Most of the time, a specific file is given precedence over a generic one.  For example, a tpl file named after a specific field will be given precedence over a generic tpl for a _type_ of field.  If you have an image field named "my\_image", then file `tpls/fields/elements/my_image.tpl` will be used to format all instances of that field, whereas another image field without a specific named tpl will default to the generic `tpls/fields/elements/_image.tpl` catchall.


### tpls/fields/elements ###

This directory contains tpls used to format instances of a type of field, e.g. `_text.tpl`, `_textarea.tpl`, `_dropdown.tpl`, etc.  These tpls are only used if there is no dedicated tpl for a given field inside of the **cctm/tpls/fields/elements** directory.   These are probably what you will want to customize the most.

For example, if you wanted _all_ WYSIWYG fields to use a particular bit of HTML and CSS, you can place a file named `_wysiwyg.tpl` inside the **cctm/tpls/fields/elements** directory and your user-created `_wysiwyg.tpl` file will be used to format all instances of WYSIWYG fields.


### tpls/fields/wrappers ###

The wrapper tpls wrap field instances.  Usually a wrapper tpl will wrap multiple field instances (as in a "repeatable" field), but sometimes they wrap individual field instances too.  The wrapper tpl is usually where you will find Javascript controls or CSS divs that encapsulate all instances of a field.

![https://img.skitch.com/20111222-bhi4js41kbsqrqm4g5h888ipj1.jpg](https://img.skitch.com/20111222-bhi4js41kbsqrqm4g5h888ipj1.jpg)

In the example above, you can see how the wrapper tpl surrounds all instances of the field tpls: the buttons are contained in the wrapper tpl.

### tpls/post\_selector ###

The post-selector is what pops up in a thickbox when you select a relation, image, or media field.  The tpls inside this directory are used to format the results displayed in the thickbox search results.

### tpls/samples ###

This is only used to generate sample WordPress template files; you will probably never need to modify these formatting templates.

### tpls/metaboxes ###

Starting with version 0.9.8, you can add metaboxes and customize the HTML.  Normally, your HTML should include just the `[+content+]` placeholder, but you may also wish to define a [Static Metabox](StaticMetabox.md): i.e. a metabox that contains only HTML text and no field elements.

**Available Placeholders:**

  * `[+content+]` : contains all the fields generated by the normal processes.
  * `[+nonce+]` : contains the special WordPress nonce field, used for security.
  * `[+your_custom_field+]` : each field assigned to a metabox has a placeholder devoted to it.

Remember: you should use _either_ the `[+content+]` placeholder OR both the individual field placeholders AND the `[+nonce+]` placeholder.

# Tutorial #

<a href='http://www.youtube.com/watch?feature=player_embedded&v=VmSxzuQd7N8' target='_blank'><img src='http://img.youtube.com/vi/VmSxzuQd7N8/0.jpg' width='425' height=344 /></a>

Below are the steps outlined in the video:

  1. Create a custom image field and add it to one of your post-types.
  1. Create the following directories (note that the structure mirrors that of the CCTM's `tpls` directory):
    * `wp-content/uploads/cctm/tpls/fields/elements`
    * `wp-content/uploads/cctm/tpls/fields/wrappers`
  1. Copy the following files:
    * `wp-content/plugins/custom-content-type-manager/tpls/fields/elements/_image.tpl` **to** `wp-content/uploads/cctm/tpls/fields/elements/_image.tpl`
    * `wp-content/plugins/custom-content-type-manager/tpls/fields/wrappers/_relation.tpl` **to** `wp-content/uploads/cctm/tpls/fields/wrappers/_relation.tpl`
  1. Rename the following file `wp-content/uploads/cctm/tpls/fields/wrappers/_relation.tpl` to `_image.tpl` -- this step is optional, but it helps show the priority of which tpls get used.
  1. Edit the copies of your tpls!  The `elements/_image.tpl` formatting template will be used to format the individual image selection for all image fields.  The `wrappers/_image.tpl` is used to format the wrapping template of all image fields.
  1. Make a copy of the `wrappers/_image.tpl` and name the copy after the name of the field, e.g. name the copy `wrappers/poster_img.tpl`.   Now the `wrappers/poster_img.tpl` will be used to wrap all instances of the **poster\_img** field, whereas the catch-all `wrappers/_image.tpl` will be used to wrap all generic image fields.


| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | Use the `[+help+]` placeholder to display all possible placeholders available in any given .tpl file. |
|:--------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------|