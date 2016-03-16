The CCTM relies on string parsing to do automated find-and-replace operations on formatting strings.  It converts strings such as

```
Hello [+first_name+], we have confirmed your trip to [+city+] on [+date+].
```

to something like

```
Hello Steve, we have confirmed your trip to New York on March 17, 2013.
```


# Placeholders #

## Simple Placeholders ##

Basic placeholders begin with a `[+` and end with a `+]`, e.g. `[+post_title+]`.  Starting in version 0.9.6.1 of the CCTM, complex placeholders were supported due to enhancements in the parser functionality.  [Output Filters](OutputFilters.md) can referenced in the placeholders, and the syntax is similar to what is supported by the [get\_custom\_field()](get_custom_field.md) and [print\_custom\_field()](print_custom_field.md) functions.


### Sample Placeholders ###

The most basic examples match a placeholder to a named key (e.g. the name of a custom field):

```
[+first_name+]
```


Starting with version 0.9.6.1 of the CCTM, you can use advanced placeholders in your formatting tpls in the following syntax:

```
[+placeholder_name:outputfilter==option1||option2+]
```

This is useful when the raw value is not in a format that is useful, e.g. when a relation field stores a reference to a post ID, or an image to an attachment id:

```
[+my_image_id:to_image_tag+]
```

```
[+related_post:to_link+]
```

### Nested Tags ###

Sometimes you need to supply placeholders as arguments to output filters.  Because the CCTM Parser is _not_ iterative, we use different glyphs to represent these "2nd level" placeholders.

```
[+related_post:get_post==My post title is {{post_title}}+]
```



---


# Function #

The parsing behavior is implemented by the `CCTM::parse` function.

_string_ parse(string _$tpl_, array _$hash_ `[`, boolean _$preserve\_unused\_placeholders_`]`)

  * **$tpl** the formatting string containing `[+placeholders+]`
  * **$hash** an array of key/value pairs, with the key names corresponding to the placeholder names
  * **$preserve\_unused\_placeholders** optional boolean that controls whether the output string removes or preserves any unused placeholders.  Default: false (all placeholders are removed).

## Example ##

```
$tpl = 'Hello [+first_name+], we have confirmed your trip to [+city+] on [+date+].';
$hash = array(
    'first_name' => 'Steve',
    'city' => 'New York',
    'date' => '2013-03-17'
);
print CCTM::parse($tpl, $hash);

// prints :
// Hello Steve, we have confirmed your trip to New York on 2013-03-17.
```


---


# Limitations #

Although placeholders go a long way in keeping your view and logic layers separated, they are not a perfect solution.  If any output filter returns an array, the parser won't know what to do with that value: your results may be unpredictable.  In general, if you're getting this deep into the Parser behavior, you should consider using PHP as an alternative.

**Advantages**

  * Formatting strings are easily cached.
  * They cannot crash or be hacked.
  * Since they are static strings with only limited "windows" into dynamic functionality, they are far less likely to devolve into spaghetti.

**Disadvantages**

  * They are not PHP
  * They return raw values: any manipulation of the values must occur via an Output Filter
  * They use their own custom syntax