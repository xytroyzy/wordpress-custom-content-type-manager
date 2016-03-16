

# Description #

The **wrapper** filter will wrap non-empty input. This allows you add extra markup to non-empty values and let empty values pass through. Pass this filter either a formatting string that uses the `[+content+]`; placeholder, or supply a 2 element array that specifies text that will appear before and after the input text.  See the examples below.

# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$input_ `[`, mixed _$options_ `]` )

  * **$input**: _string_ a post ID
  * **$options**: _mixed_. You can supply either a formatting string or a 2 element array.
  * **OUTPUT**: _string_. Formatted string -- the formatting string with the input replaced.

# Options #

  * **string**: Supply a formatting string that contains the `[+content+]` placeholder, e.g.

> `<span class="my_class"><strong>Fax:</strong> [+content+]</span>`

  * **array**: If you supply an array, the first element in the array will be placed _before_ the content, the 2nd element in the array will be placed _after_ the content, e.g.

> `array('<span class="my_class"><strong>Fax:</strong> ', '</span>')`

## Example 1 ##

If your custom field is empty, the wrapper filter will not append anything to the output.  Imagine the following page that lists contact information:

```
<?php
print_custom_field('phone_number:wrapper', array('<span class="my_class"><strong>Phone:</strong> ', '</span>') );

print_custom_field('fax_number:wrapper', array('<span class="my_class"><strong>Fax:</strong> ', '</span>') );
?>
```

If the record being formatted has a phone number, but no fax number, the generated output might be something like this:

```
<span class="my_class"><strong>Phone:</strong>800-555-1212</span>
```

The **phone\_number** was wrapped because it had a value, but the the **fax\_number** was _not_ wrapped because it was empty.

## Example 2 ##

You can also use a formatting string to format the output.  Simply ensure that your string contains the  `[+content+]` placeholder to designate where the input value will appear in the output:

```
<?php
print_custom_field('fax_number:wrapper', '<span class="my_class"><strong>Fax:</strong> [+content+]</span>');
?>
```

# See Also #