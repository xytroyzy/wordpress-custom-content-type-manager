

# Description #

The _email_ filter obscures an email address so it is still readable by a human, but more difficult for it to be harvested by a spam-bot.


| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | <font color='red'>WARNING: this filter is not guaranteed to prevent an email address from being harvested!</font>   |
|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------|

# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$fieldname_ `[`, mixed _$options_ `]` )

  * **$fieldname**: _string_ field name `+ :email`
  * **$options**: _none_.
  * **OUTPUT**: _string_.  An encoded version of the $input.

## Example 1 ##

```
<p>Contact me at the following address:</p>
<?php print_custom_field('my_email:email'); ?>
```

## Example 2 (array) ##

If you've got a repeatable field containing an array of email addresses, you should iterate over the results and format each.

```
<p>Contact one of us at one of the following email addresses:</p>
<?php 

$email_array = get_custom_field('my_email:email'); 
foreach ($email_array as $email) {
    print $email . '<br/>';
}
?>
```

# See Also #

See David Walsh's post for a description of this technique: http://davidwalsh.name/php-email-encode-prevent-spam

For a more thorough tactic, look at Dan Benjamin's Hivelogic Enkoder: http://hivelogic.com/enkoder/