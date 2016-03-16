

# Description #

Starting with version 0.9.5 of the CCTM, developers can write their own [Output Filters](OutputFilters.md).  All that's required is that you write a PHP class that extends the [CCTM\_OutputFilter](CustomOutputFilters.md) class.  That class is an [abstract class](http://php.net/manual/en/language.oop5.abstract.php).  If you haven't used abstract classes before, check out Aleem Bawany's helpful explanation of them: [Abstract Classes in PHP](http://aleembawany.com/2010/04/03/understanding-abstract-classes-in-php/).  In a nutshell, they allow me to define what a class should look like, then the child class must implement the necessary functions.  That means each Output Filter follows a pattern: each class has a predictable set of functions that the CCTM expects.

You can see examples of the built-in Output Filters inside of **custom-content-type-manager/includes/filters**.

Developers can save their own filters inside of **wp-content/uploads/cctm/filters**


---


# The Base Class #

By looking at the base class inside of **wp-content/plugins/custom-content-type-manager/includes/CCTM\_OutputFilter.php**, you can see the following variables and methods.  Here's a brief description of each variable and function.

### <font color='green'>public $show_in_menus</font> ###

_boolean_ : whether or not the filter should be listed in the field definitions as a selectable filter.

### <font color='green'>public function filter()</font> ###

_mixed_ **filter**( mixed _$input_ , mixed _$options_)

This is where the magic happens.  This is where you put the code that will modify (i.e. filter) the input and return the modified output.  The $input is either the raw input coming from the [get\_custom\_field()](get_custom_field.md) function or if you're chaining Output Filters together, the input to this function will be the output from the previous function.

If your filter takes any arguments, they'll be sent to the $options argument.


### <font color='green'>public function get_description()</font> ###

_string_ **get\_description**()

Get the human readable description of what it is your filter does.

### <font color='green'>public function get_example()</font> ###

_string_ **get\_example**()

Show the user how to use the filter inside a template file.

### <font color='green'>public function get_name()</font> ###

_string_ **get\_name**()

Returns the human-readable name of the filter.

### <font color='green'>public function get_url()</font> ###
_string_ **get\_url**()

Returns the URL where the user can read more about the filter.


---


# Sample Output Filter Class #

So that's all well and good, but let's have a look at how we might actually write our own filter.  Create a file named **demo.php** inside of **wp-content/uploads/cctm/filters** (create the directories if they don't already exist.

## Naming Conventions ##

  * Name your files in _all lowercase_!
  * Do not use any spaces or dashes in the name!
  * The name of the class _must_ correspond exactly to the base name of the file.  The classname will have a prefix of `CCTM_` and it will not use the ".php" extension.  E.g. if your filter is named `my_filter`, then your filename would be `my_filter.php` and your classname will be `CCTM_my_filter`.

## Sample Class ##

Create a file named **demo.php** and save it into **wp-content/uploads/cctm/filters**.  Paste the following code:

```
<?php
/**
 * @package CCTM
 * 
 * Just a demonstration of a custom output filter
 */

class CCTM_demo extends CCTM_OutputFilter {

	/**
	 * Apply the filter.
	 *
	 * @param 	string 	input
	 * @param	 mixed	optional arguments to return if the input is empty
	 * @return string
	 */
	public function filter($input, $options=null) {
		return $input . ' ZORRO WAS HERE!!!';
	}


	/**
	 * @return string	a description of what the filter is and does.
	 */
	public function get_description() {
		return 'The <em>demo</em> filter appends "ZORRO WAS HERE!!!" to everything it filters.';
	}


	/**
	 * Show the user how to use the filter inside a template file. Use the $fieldtype
	 * argument if you need to alter your examples depending on which type of field
	 * is being filtered.  Use the $is_repeatable value to show more thorough, e.g. when
	 * usage may change depending on whether the field represents a single value or a list of values.
	 *
	 * @param string $fieldname -- the name of the field
	 * @param string $fieldtype -- the type of field. 
	 * @param boolean $is_repeatable -- whether or not the instance is a repeatable field.
	 * @return string 	a code sample 
	 */
	public function get_example($fieldname='my_field',$fieldtype,$is_repeatable) {
		return "<?php print_custom_field('$fieldname:demo'); ?>";
	}

	/**
	 * @return string	the human-readable name of the filter.
	 */
	public function get_name() {
		return 'Demo';
	}

	/**
	 * @return string	the URL where the user can read more about the filter
	 */
	public function get_url() {
		return 'http://code.google.com/p/wordpress-custom-content-type-manager/wiki/OutputFilters';
	}
}
/*EOF*/

```

That's it.  We didn't have to use the **$show\_in\_menus** variable, because we're fine with the default (true).


---


# Putting it to Use #

Once you've created your new Output Filter, you can access it in several places:

### Inside your Custom Field Definitions ###

When you create or edit a custom field definition, your new Output Filter will be available in the dropdown of available Output Filters (unless you set **$show\_in\_menus** to **false**):

![https://img.skitch.com/20111208-b2auqm82uhciyit65bf3u8yfgp.jpg](https://img.skitch.com/20111208-b2auqm82uhciyit65bf3u8yfgp.jpg)


### Inside your Template Files ###

You can now modify the output of your custom fields using your filter by specifying it by its short name.

![https://img.skitch.com/20111208-byt1e9wnxcky48r1q2e2jw7nns.jpg](https://img.skitch.com/20111208-byt1e9wnxcky48r1q2e2jw7nns.jpg)

Depending on what your filter does, you may want to use [get\_custom\_field()](get_custom_field.md) instead of [print\_custom\_field()](print_custom_field.md).

### Listing by the help filter ###

If you haven't made use of the [help](help_OutputFilter.md) Output Filter, you can appreciate it more now that you've written your own filter.  Apply the **help** filter to _any_ instance of [get\_custom\_field()](get_custom_field.md) or [print\_custom\_field()](print_custom_field.md), and you'll get a full readout of all available Output Filters and how to use them.

For example, adding the following to a template file produces output that includes a blurb about our new Output Filter:

`<?php print_custom_field('my_field:help'); ?>`

![https://img.skitch.com/20111208-th28ww1cghgi786txb2rw9ppkc.jpg](https://img.skitch.com/20111208-th28ww1cghgi786txb2rw9ppkc.jpg)