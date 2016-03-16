

# Description #

Starting with version 0.9.5 of the CCTM, developers can write their own custom [Validators](Validators.md).  All that's required is that you write a PHP class that extends the [CCTM\_Validator](Validators.md) class.  That class is an [abstract class](http://php.net/manual/en/language.oop5.abstract.php).

You can see examples of the built-in Output Filters inside of **custom-content-type-manager/includes/validators**.

Developers can save their own filters inside of **wp-content/uploads/cctm/validators**

A couple things to remember:

  * A field is only validated if it is _NOT EMPTY_.  Empty fields are considered valid unless you have checked the "Required" checkbox.
  * For repeatable fields (i.e. for fields that store a list of values), each input is validated individually.  The code in each validator is meant to be as simple as possible, so it will always validate a single string (not an array).


---


# The Base Class #

By looking at the base class inside of **wp-content/plugins/custom-content-type-manager/includes/CCTM\_Validator.php**, you can see the following variables and methods.  Here's a brief description of each variable and function.

### <font color='green'>public $show_in_menus</font> ###

_boolean_ : whether or not the validator should be listed in the field definitions as a selectable validator.  99% of the time, this will be **true**, but if you've got some creative reason that you want your validator to only be available programmatically and remain invisible to the GUI, you can set this to false (Note: I can't think of a use-case for this to be false, but if you can, knock yourself out!)

### <font color='red'>public function get_description()</font> ###

_string_ **get\_description**()

Get the human readable description of what your validator does.


### <font color='red'>public function get_name()</font> ###

_string_ **get\_name**()

Get the name of the validator.  This string can be localized.


### <font color='green'>public function get_options_html()</font> ###

_string_ **get\_options\_html**()

Optionally, you can use this function to generate any HTML for your validator.  This HTML is what shows up when you select a validator when you are defining a custom field.  See the [Number](number_Validator.md) validator (`validators/number.php`) for an example of a class that implements some options.


### <font color='red'>public function validate()</font> ###
_string_ **validate**(string $input)

This is where the magic happens.  The code inside this function should test the `$input` to see if it is valid. If the `$input` is not valid, an error message should be written to the `$this->error_msg` variable.  If the `$this->error_msg` variable remains empty, the $input is considered valid.

The $input can be modified before being returned (e.g. to suggest a correction), but normally, you'll want to return the $input exactly as you found it.

You will never have to test an `$input` to see whether or not it is empty: this is handled for you.  Inputs are passed along to be validated when they are _not empty_.  Also, the `$input` will always be singular (a string), not an array.

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | The input to the **validate()** function will always be a string (not an array).  If the validation rule is used on a repeatable field (i.e. one storing multiple values), then each value will be sent to the **validate()** function individually. |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|


---


# Example 1 #

If you want create your own validation function, you can add a file to `wp-content/uploads/cctm/validators/`.  Do NOT add files to the `plugins/custom-content-type-manager/validators/` directory!  It will get overwritten when you update the plugin!

## Naming Conventions ##

  * Name your files in _all lowercase_!
  * Do not use any spaces or dashes in the name!
  * The name of the class _must_ correspond exactly to the base name of the file.  The classname will have a prefix of `CCTM_Rule_` and it will not use the ".php" extension.  E.g. if your validator is named `my_validator`, then your filename would be `my_validator.php` and your classname will be `CCTM_Rule_my_validator`.

Create a file named **vowels\_only.php** and paste the following into it:

```
<?php
/**
 * Check to see if the input contains only Vowels (AEIOU).
 * @package CCTM_Validator
 *
 */
class CCTM_Rule_vowels_only extends CCTM_Validator {


	/**
	 * @return string	a description of what the validation rule is and does.
	 */
	public function get_description() {
		return __('Check that the input contains only vowels.', CCTM_TXTDOMAIN);		
	}


	/**
	 * @return string	the human-readable name of the validation rules.
	 */
	public function get_name() {
		return __('Vowels Only', CCTM_TXTDOMAIN);
	}
		
	/**
	 * Run the rule: check the user input. Return the (filtered) value that should
	 * be used to repopulate the form.
	 *
	 * @param string 	$input (as it is stored in the database)
	 * @return string
	 */
	public function validate($input) {
		if (preg_match("/[^aeiou]$/i", $input)) {
			$this->error_msg = sprintf(__('The %s field can only contain vowels.', CCTM_TXTDOMAIN), $this->get_subject());
		}
		
		return $input;
	}
	
}
/*EOF*/
```

After adding this, you may need to clear the CCTM's cache by clicking on the **CCTM --> Clear Cache** menu option.

This example validator will ensure that inputs contain only vowels.  You can see that the real action here occurs in the `validate()` function.  That's where your regexes etc. will be.