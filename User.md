|![https://img.skitch.com/20120129-b68tkm2edkau7yaarj8187519x.jpg](https://img.skitch.com/20120129-b68tkm2edkau7yaarj8187519x.jpg)|A user field allows you to select a user from your local site. (Added 0.9.5.6)|
|:--------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------|

User fields are used to store a foreign key reference to a user.  Because the raw user ID is not terribly useful in and of itself, user fields most often will most often rely on the [userinfo](userinfo_OutputFilter.md) output filter to translate and format the information represented by the user ID.




---


## Field Definition (Meta Data) ##

The field definition affects how instances of this field type will display in the WordPress manager while creating or posts that contain instances of this field type.

  * **label** : the label identifies this field when you create or edit a post in the WordPress manager
  * **name** : the name is a unique identifier used when saving values for instances of this field; it corresponds to the **meta\_key** column **wp\_postmeta** table.  You will use this name when retrieving field values inside of your themes via the `print_custom_field()` or `get_custom_field()` functions.
  * **description** : the description provides extra information to users as they create or edit a post in the WordPress manager.
  * **class** : this affects the CSS class in the WordPress manager that is used to display instances of this field type.
  * **extra** : this can be used to provide additional parameters to the text input, e.g. `size="10"` or custom javascript.
  * **default\_value** : This is used to determine the default value for new instances of this field type.
  * **output\_filter** : [Output Filters](OutputFilters.md) control how data is filtered before being sent to your theme files. The recommended default output filter for a user field is _userinfo_


---


## Included Javascript/CSS files ##

  * _none_


---


## Example of Use  in Template File ##


### Example 1: Retrieve the src of the Image ###

Here we show how to use the [to\_image\_src](to_image_src_OutputFilter.md) Output Filter:

```
<?php 
$userinfo = get_custom_field('my_user:userinfo'); 
print_r($userinfo); // see what info is available!
?>
```

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | In the case of a user field with no output filter, `print_custom_field('fieldname');` will return _an integer number_ representing the ID of the user that has been referenced!  |
|:--------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|


### Recommended Output Filters ###

  * [userinfo](userinfo_OutputFilter.md)


---


## Customizing Manager HTML ##

See [CustomizingManagerHTML](CustomizingManagerHTML.md) for more information on customizing the appearance of the manager HTML.

#### Option tpl ####

The option tpl is used to format the option elements in the dropdown.

  1. `fields/elements/{fieldname}.tpl`
  1. `fields/elements/_user.tpl`

#### Field tpl ####

The first of following tpls found will be used to format the field:

  1. `fields/elements/{fieldname}.tpl`
  1. `fields/elements/_user.tpl`
  1. `fields/elements/_default.tpl`

#### Wrapper tpl ####

The first of the following tpls found will be used to wrap the output:

  1. `fields/wrappers/{fieldname}.tpl`
  1. `fields/wrappers/_user.tpl`
  1. `fields/wrappers/_user.tpl`

### Multi ###

User fields can be repeatable.  If the "repeatable" option is checked, the following tpls are used to format the output:


#### Option tpl ####

The option tpl is used to format the option elements in the dropdown.

  1. `fields/elements/{fieldname}.tpl`
  1. `fields/elements/_multi_user.tpl`


#### Field tpl ####

The first of following tpls found will be used to format the field:

  1. `fields/elements/{fieldname}.tpl`
  1. `fields/elements/_user_multi.tpl`

#### Wrapper tpl ####

The first of the following tpls found will be used to wrap the output:

  1. `fields/wrappers/{fieldname}.tpl`
  1. `fields/wrappers/_user_multi.tpl`