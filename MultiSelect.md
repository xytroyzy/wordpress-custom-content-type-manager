|![http://s8.postimage.org/hud1ssltt/multiselect.png](http://s8.postimage.org/hud1ssltt/multiselect.png)|Multi-select fields are used to store a list of multiple values. The values are encoded as a JSON array.|
|:------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------|





---


## Sample Screenshots ##

![https://img.skitch.com/20111223-p1mn8mj7qaum6cffqnkjhkdbxg.jpg](https://img.skitch.com/20111223-p1mn8mj7qaum6cffqnkjhkdbxg.jpg)


---


## Field Definition (Meta Data) ##

The field definition affects how instances of this field type will display in the WordPress manager while creating or posts that contain instances of this field type.

  * **label** : the label identifies this field when you create or edit a post in the WordPress manager
  * **name** : the name is a unique identifier used when saving values for instances of this field; it corresponds to the **meta\_key** column **wp\_postmeta** table.  You will use this name when retrieving field values inside of your themes via the `print_custom_field()` or `get_custom_field()` functions.
  * **description** : the description provides extra information to users as they create or edit a post in the WordPress manager.
  * **class** : this affects the CSS class in the WordPress manager that is used to display instances of this field type.
  * **extra** : this can be used to provide additional parameters to the text input, e.g. `size="10"` or custom javascript..
  * **default\_value** : This is used to determine the default value for new instances of this field type.
  * **output\_filter** : [Output Filters](OutputFilters.md) -- you'll need to use a filter that handles arrays, e.g. [to\_array](to_array_OutputFilter.md), or [formatted\_list](formatted_list_OutputFilter.md)
  * **alternate\_input** : textarea -- starting with version 0.9.6.5, users can supply a block of text with each line representing an option (like Drupal), or a valid SQL query that should drive the available options.
  * **is\_sql** : checkbox (1|0) -- if checked, the text in the alternate\_input will be evaluated as a database query.


---


## Alternate Inputs ##

The dropdown fields are versatile and powerful.  Starting with 0.9.6.5, you can add options in bulk by pasting them into the Alternate Input field.  The format is simple: each line becomes a new option.

**Alternate Input Example 1:**

For simple bulk entry, paste all your options into the text area: each line will represent a new option.
```
Scott
Mark
Bruce
Kevin
Dave
```



**Alternate Input Example 2:**

If you wish to have separate options and values, then use two pipe characters "||" to separate the visible option label on the left from the stored value on the right.  Values are trimmed, so you can use some space around your double-pipes.  It is valid to include the double-pipes on some lines and omit it on others.

```
Big Bear || big_bear
Little Dog || litte_bear
Cats
```


**MySQL Query**

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

  * `/js/dropdown.js`  (Requires jquery)


---


## Example of Use  in Template File ##

MultiSelects always contain an array of data, so you always have to handle them as an array of data.

### Example 1: Simple Array ###

```
<?php 
$my_array = get_custom_field('my_multiselect:to_array'); 
foreach ($my_array as $item) {
    print $item . '<br/>';
}
?>
```

### Example 2: Formatted List ###

```
<?php 
print_custom_field('my_multiselect:formatted_list', ','); 
// will join each value with a comma, e.g. "dog, cat, man"
?>
```

### Example 3: Another Formatted List ###

```
print_custom_field('my_multiselect:formatted_list', array('<li>[+value+]</li>','<ul>[+content+]</ul>') );
/* prints a formated unordered list:
<ul>
  <li>man</li>
  <li>bear</li>
  <li>pig</li>
</ul>
*/
```




### Recommended Output Filters ###

  * [to\_array](to_array_OutputFilter.md)
  * [formatted\_list](formatted_list_OutputFilter.md)


---


## Customizing Manager HTML ##

MultiSelect fields are the odd duck here.  THey use the 2 tpls: option and wrapper.  See [CustomizingManagerHTML](CustomizingManagerHTML.md) for more information.

#### Option tpl ####

The first of following tpls found will be used to format the field:

  * `fields/options/{fieldname}.tpl`
  * `fields/options/_multiselect.tpl`
  * `fields/options/_checkbox.tpl`

#### Wrapper tpl ####

The first of the following tpls found will be used to wrap the output:

  1. `fields/wrappers/{fieldname}.tpl`
  1. `fields/wrappers/_multiselect.tpl`
  1. `fields/wrappers/_default.tpl`