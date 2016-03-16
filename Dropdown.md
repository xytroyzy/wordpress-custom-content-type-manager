|![http://s9.postimage.org/smg6bx11n/dropdown.png](http://s9.postimage.org/smg6bx11n/dropdown.png)|Dropdown fields let you select a single option from a list of options that the user defines one-by-one, in bulk, or via s SQL database query.|
|:------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------|




---


## Sample Image ##

### Dropdown Format ###

![https://img.skitch.com/20111223-pttjqhfwnaf1691k4wp8qbwb6r.jpg](https://img.skitch.com/20111223-pttjqhfwnaf1691k4wp8qbwb6r.jpg)

### Radio Button Format ###

![https://img.skitch.com/20111223-mdfrnurxrd3qfqussncakssnq1.jpg](https://img.skitch.com/20111223-mdfrnurxrd3qfqussncakssnq1.jpg)


---


## Field Definition (Meta Data) ##

The field definition affects how instances of this field type will display in the WordPress manager while creating or posts that contain instances of this field type.

  * **label** : the label identifies this field when you create or edit a post in the WordPress manager
  * **name** : the name is a unique identifier used when saving values for instances of this field; it corresponds to the **meta\_key** column **wp\_postmeta** table.  You will use this name when retrieving field values inside of your themes via the `print_custom_field()` or `get_custom_field()` functions.
  * **description** : the description provides extra information to users as they create or edit a post in the WordPress manager.
  * **class** : this affects the CSS class in the WordPress manager that is used to display instances of this field type.
  * **extra** : this can be used to provide additional parameters to the text input, e.g. custom javascript..
  * **default\_value** : this should correspond to one of the defined options
  * **options** : an array of selectable options
  * **values** : an array of values to be stored.  If _use\_key\_values_ is checked, these will be distinct from the _options_, otherwise this array will be exactly equal to the _options_ array.
  * **use\_key\_values** : 0|1. Checkbox defining whether or not you are creating a simple dropdown (0) where the options displayed visibly match the values stored, or if you are doing something more complex (1) where the values stored are different from what's displayed to the user.
  * **display\_type** : dropdown|radio -- this defines whether or not the field will be rendered as a dropdown field or as a series of radio buttons.
  * **alternate\_input** : textarea -- starting with version 0.9.6.5, users can supply a block of text with each line representing an option (like Drupal), or a valid SQL query that should drive the available options.
  * **is\_sql** : checkbox (1|0) -- if checked, the text in the alternate\_input will be evaluated as a database query.


---


## Alternate Inputs ##

The dropdown fields are versatile and powerful.  Starting with 0.9.6.5, you can add options in bulk by pasting them into the Alternate Input field.  The format is simple: each line becomes a new option.

**Alternate Input Example 1:**
```
Scott
Mark
Bruce
Kevin
Dave
```


If you wish to have separate options and values, then use two pipe characters "||" to separate the visible option label on the left from the stored value on the right.  Values are trimmed, so you can use some space around your double-pipes.  It is valid to include the double-pipes on some lines and omit it on others.

**Alternate Input Example 2:**
```
Big Bear || big_bear
Little Dog || litte_bear
Cats
```

Lastly, you can check the box labeled "Execute as a MySQL query" if you have instead entered a SQL database query.  Note that the following behaviors of the database query option:

  * Only the first 2 columns selected are relevant, so `SELECT *` operations are discouraged.  The first column in each result will be used as the visible option label.  The second column (if present) will be the value stored in the database.
  * The database queried is the same one used by the WordPress installation.
  * You may use the `[+table_prefix+]` placeholder to dynamically detect the table prefix used on local WordPress installation.  Using this placeholder is recommended over hard-coding table names because 1. your table prefix may change and 2. if you migrate your CCTM definitions to a new site, the table prefix may be different.

**Alternate Input Example 3:**
```
SELECT post_title, ID 
FROM [+table_prefix+]posts 
WHERE post_type='movie'
AND post_author=3
```

Be advised that !MySQL queries are dependent on the specific _data_ in your site, so migrating your CCTM definitions to a new installation with different data may result in broken queries and broken dropdowns.


---


## Included Javascript/CSS files ##

  * `/js/dropdown.js` (Requires jQuery)


---


## Example of Use  in Template File ##

The output of `print_custom_field()` will simply print the selected option.  Note that the exact value returned will depend on whether or not you have checked "Distinct options/values" in your definition.


```
<?php
print_custom_field('my_dropdown'); 
?>
```

### Recommended Output Filters ###

  * [default](default_OutputFilter.md)
  * [wrapper](wrapper_OutputFilter.md)



---


## Customizing Manager HTML ##

Dropdown fields have wrapper, field, and option formatting templates, and there are 2 sets available depending on whether you format your output as a regular dropdown or as a series of radio buttons.

See [CustomizingManagerHTML](CustomizingManagerHTML.md) for more information on customizing the appearance of the manager HTML.

### Regular Dropdown Format ###

#### Field tpl ####

The first of following tpls found will be used to format the field:

  1. `fields/elements/{fieldname}.tpl`
  1. `fields/elements/_dropdown.tpl`
  1. `fields/elements/_default.tpl`

#### Wrapper tpl ####

The first of the following tpls found will be used to wrap the output:

  1. `fields/wrappers/{fieldname}.tpl`
  1. `fields/wrappers/_dropdown.tpl`
  1. `fields/wrappers/_default.tpl`


#### Option tpl ####

The first of the following tpls found will be used to wrap the output:

  1. `fields/options/{fieldname}.tpl`
  1. `fields/options/_option.tpl`


### Radio Button Format ###

#### Field tpl ####

The first of following tpls found will be used to format the field:

  1. `fields/elements/{fieldname}.tpl`
  1. `fields/elements/_radio.tpl`
  1. `fields/elements/_default.tpl`

#### Wrapper tpl ####

The first of the following tpls found will be used to wrap the output:

  1. `fields/wrappers/{fieldname}.tpl`
  1. `fields/wrappers/_radio.tpl`
  1. `fields/wrappers/_default.tpl`


#### Option tpl ####

The first of the following tpls found will be used to wrap the output:

  1. `fields/options/{fieldname}.tpl`
  1. `fields/options/_radio.tpl`


Dropdown fields do not have a "repeatable" option, so there is not a separate set of "multi" tpls.  Use a Multiselect field to achieve a "repeatable dropdown".