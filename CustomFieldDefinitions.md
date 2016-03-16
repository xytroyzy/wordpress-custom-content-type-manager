This page explains what goes into a Custom Field Definition.  A field definition defines the name, data-type, and other aspects of a field.  These definitions are separate from posts or pages.  The definition itself does not store post data: the posts themselves store values, the definition only defines how that data is entered.

Each type of field may have different attributes, but there are a handful of core attributes shared by most types of fields

# Core Attributes #

---


## Label ##

The label is bolded text that tells the user what this field is.

![https://img.skitch.com/20120902-jsxs9im6mdpqfym83mykdncyg2.jpg](https://img.skitch.com/20120902-jsxs9im6mdpqfym83mykdncyg2.jpg)

## Name ##

The name is not seen directly by the user: it is a unique name for the field so that its values can be identified in the `wp_postmeta` database table.  The name is also used to generate CSS IDs for form elements are generated when a post is created or edited. The name should be all one word and lowercase, and only alphabetical characters and underscores should be used.

<font color='red'>WARNING: WYSIWYG fields do not allow underscores for field names!</font>

## Default Value ##

You can set a default value if you want new posts to start out with a given value instead of as a blank.

## Extra ##

The extra attribute is useful for adding anything extra to the field definition that should not be constrained inside double quotes. Extra text appears verbatim inside the form element, e.g. `<input type="text" my extra data goes here />`

Practical uses for this might include adding javascript events, or HTML 5 field attributes.

![https://img.skitch.com/20120902-keqafg3xqc5gituc7ii62t3n9j.jpg](https://img.skitch.com/20120902-keqafg3xqc5gituc7ii62t3n9j.jpg)

## Class ##

When the CCTM generates form elements inside the WordPress manager, it adds standard IDs and classes to identify each unique field and each type of field to make the job of styling the output easier. However, if you wish to modify the display of generated form elements inside the WordPress manager, you may wish to add your own CSS classes.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=0X9OBLUq2T4' target='_blank'><img src='http://img.youtube.com/vi/0X9OBLUq2T4/0.jpg' width='425' height=344 /></a>

## Is Repeatable ##

If checked, the field will store a _list_ of values instead of just a _single_ value.  For example an image field _without_ this checked will store only a single image, whereas an image field that is repeatable can select multiple images.

<font color='red'>WARNING: not every type of field supports this!</font>

## Description ##

This is a brief note describing what this field does.  The description appears under each instance of the field when a user creates or edits a post.

![https://img.skitch.com/20120902-jsxs9im6mdpqfym83mykdncyg2.jpg](https://img.skitch.com/20120902-jsxs9im6mdpqfym83mykdncyg2.jpg)