

# Description #

Starting with version 0.9.4, developers can create their own PHP classes that describe their own custom fields.  You can use this feature to write your own types of custom fields, just like with [Custom Output Filters](CustomOutputFilters.md).


---

# Where to Save #

Put your field classes inside your custom fields directory, which by default is `wp-content/uploads/cctm/fields`.  That directory will be scanned for additional classes.  (If you've customized the location of your uploads directory, then move the `tpls` directory into your custom location)

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | <font color='red'>WARNING</font>: do not add files to the `wp-content/plugins/custom-content-type-manager/fields` directory!  Any files you add there will get deleted when the plugin is updated!|
|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

After you've added a field class to this folder, you can clear the CCTM cache: CCTM --> clear Cache.


---


# The Base Class #

`CCTM_FormElement` is an abstract class that defines the behavior for all custom fields.

By looking at the base class inside of `wp-content/plugins/custom-content-type-manager/includes/CCTM_FormElement.php`, you can see the following variables and methods.  Here's a brief description of each relevant variable and function.

### <font color='green'>protected $props</font> ###

_array_ : an associative array that defines which properties each instance of the field will have.

### <font color='green'>public function admin_init()</font> ###

_void_ **admin\_init**()

Override this function to register any necessary CSS/JavaScript required by your field.  For examples, look at the **relation.php** field.

### <font color='green'>public function get_create_field_definition()</font> ###

_string_ **public function get\_create\_field\_definition**()

This should return (not print) form elements that handle all the controls required to define this type of field.  Normally, you shouldn't need to implement this function.  Usually implementing the **get\_edit\_field\_definition()** is enough.

### <font color='red'>abstract</font> <font color='green'>public get_edit_field_definition()</font> ###

_string_ **get\_edit\_field\_definition**($current\_values)

This should return (not print) form elements that handle all the controls required to edit the field definition.

<font color='red'>You MUST implement this function.</font>

### <font color='red'>abstract</font> <font color='green'>public function get_name();</font> ###

_string_ **get\_name**()

This should return (not print) the common name of the field, e.g. "Text", "Dropdown", etc.

<font color='red'>You MUST implement this function.</font>


### <font color='red'>abstract</font> <font color='green'>public function get_url();</font> ###

_string_ **get\_url**()

This should return (not print) a full link to where users can get information about this type of field, e.g. `http://code.google.com/p/wordpress-custom-content-type-manager/wiki/Image`

<font color='red'>You MUST implement this function.</font>


### <font color='green'>public function get_icon();</font> ###

_string_ **get\_icon**()

Return URL to a 48x48 PNG image that should represent this type of field. e.g. `http://mysite/images/coolio.png`


---


# Sample Field Class #

If you want create your own type of custom field, you can add a file to `wp-content/uploads/cctm/fields/`.  Do NOT add files to the `plugins/custom-content-type-manager/fields/` directory!  It will get overwritten when you update the plugin!


## Naming Conventions ##

  * Name your files in _all lowercase_!
  * Do not use any spaces or dashes in the name!
  * The name of the class _must_ correspond exactly to the base name of the file.  The classname will have a prefix of `CCTM_` and it will not use the ".php" extension.  E.g. if your field is named `my_field`, then your filename would be `my_field.php` and your classname will be `CCTM_my_field`.

MORE COMING... but for now, you can take a look at the text field class:

```
<?php
/**
 * CCTM_text
 *
 * Implements a simple HTML text input.
 *
 * @package CCTM_FormElement
 */


class CCTM_text extends CCTM_FormElement
{

	public $props = array(
		'label' => '',
		'name' => '',
		'description' => '',
		'class' => '',
		'extra' => '',
		'default_value' => '',
		'is_repeatable' => '',
		'output_filter' => '',
		'required' => '',
		// 'type' => '', // auto-populated: the name of the class, minus the CCTM_ prefix.
	);


	//------------------------------------------------------------------------------
	/**
	 * This function provides a name for this type of field. This should return plain
	 * text (no HTML). The returned value should be localized using the __() function.
	 *
	 * @return string
	 */
	public function get_name() {
		return __('Text', CCTM_TXTDOMAIN);
	}


	//------------------------------------------------------------------------------
	/**
	 * This function gives a description of this type of field so users will know
	 * whether or not they want to add this type of field to their custom content
	 * type. The returned value should be localized using the __() function.
	 *
	 * @return string text description
	 */
	public function get_description() {
		return __('Text fields implement the standard <input="text"> element. "Extra" parameters, e.g. "size" can be specified in the definition.', CCTM_TXTDOMAIN);
	}


	//------------------------------------------------------------------------------
	/**
	 * This function should return the URL where users can read more information about
	 * the type of field that they want to add to their post_type. The string may
	 * be localized using __() if necessary (e.g. for language-specific pages)
	 *
	 * @return string  e.g. http://www.yoursite.com/some/page.html
	 */
	public function get_url() {
		return 'http://code.google.com/p/wordpress-custom-content-type-manager/wiki/Text';
	}


	//------------------------------------------------------------------------------
	/**
	 * This is somewhat tricky if the values the user wants to store are HTML/JS.
	 * See http://www.php.net/manual/en/function.htmlspecialchars.php#99185
	 *
	 * @param mixed   $current_value current value for this field.
	 * @return string
	 */
	public function get_edit_field_instance($current_value) {

		// Populate the values (i.e. properties) of this field
		$this->id      = $this->name;

		$fieldtpl = '';
		$wrappertpl = '';

		if ($this->is_repeatable) {
			$fieldtpl = CCTM::load_tpl(
				array('fields/elements/'.$this->name.'.tpl'
					, 'fields/elements/_'.$this->type.'_multi.tpl'
					, 'fields/elements/_default.tpl'
				)
			);
			$wrappertpl = CCTM::load_tpl(
				array('fields/wrappers/'.$this->name.'.tpl'
					, 'fields/wrappers/_'.$this->type.'_multi.tpl'
					, 'fields/wrappers/_default.tpl'
				)
			);

			$this->i = 0;
			$values = $this->get_value($current_value,'to_array');
			//die(print_r($values,true));
			$content = '';
			foreach ($values as $v) {
				$this->value = htmlspecialchars( html_entity_decode($v) );
				$this->content .= CCTM::parse($fieldtpl, $this->get_props());
				$this->i   = $this->i + 1;
			}
		}
		// Normal text field
		else {
			$this->value  = htmlspecialchars( html_entity_decode($this->get_value($current_value,'to_string') ));

			$fieldtpl = CCTM::load_tpl(
				array('fields/elements/'.$this->name.'.tpl'
					, 'fields/elements/_'.$this->type.'.tpl'
					, 'fields/elements/_default.tpl'
				)
			);
			$wrappertpl = CCTM::load_tpl(
				array('fields/wrappers/'.$this->name.'.tpl'
					, 'fields/wrappers/_'.$this->type.'.tpl'
					, 'fields/wrappers/_default.tpl'
				)
			);
			$this->content = CCTM::parse($fieldtpl, $this->get_props());
		}


		$this->add_label = __('Add', CCTM_TXTDOMAIN);
		return CCTM::parse($wrappertpl, $this->get_props());
	}


	//------------------------------------------------------------------------------
	/**
	 *
	 *
	 * @param mixed   $def field definition; see the $props array
	 * @return unknown
	 */
	public function get_edit_field_definition($def) {
		// Standard
		$out = $this->format_standard_fields($def);
		
		// Validations / Required
		$out .= $this->format_validators($def);

		// Output Filter
		$out .= $this->format_available_output_filters($def);

		return $out;
	}


}


/*EOF*/
```