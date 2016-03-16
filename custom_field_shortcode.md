# Introduction #

You can add a **custom\_field** shortcode to your main content block in order to print the values of custom fields.

```
[custom_field name="my_custom_field"]
```

## Parameters ##

  * **name**: the unique name of the custom field
  * **filter**: optional name of an output filter to use

You can pass a simple argument to the selected output filter by wrapping the options in a full tag, e.g.

```
[custom_field name="my_custom_field" filter="wrapper"]<strong>My content is:</strong> [+content+][/custom_field]
```


Warning!  Do not reference the custom field where this shortcode is placed: doing so will cause a loop.

# Since #

Version 0.9.5.10