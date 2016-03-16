

# Data Structure #

This is the proposed data structure used in version 0.9.4 and after.

```
Array(
  // each post_type def is a structure that is passable to register_post_type()
   [post_type_defs] =>
	Array
	(
	    [student] => Array
	        (
	            [cctm_hierarchical_custom] => 
	            [supports] => Array
	                (
	                    [0] => title
	                    [1] => editor
	                    [2] => page-attributes
	                )
	 
	            [taxonomies] => Array
	                (
	                )
	 
	            [post_type] => student
	            [labels] => Array
	                (
	                    [menu_name] => Students
	                    [singular_name] => student
	                    [add_new] => Add New
	                    [add_new_item] => Add New student
	                    [edit_item] => Edit student
	                    [new_item] => New student
	                    [view_item] => View student
	                    [search_items] => Search Students
	                    [not_found] => No students found
	                    [not_found_in_trash] => No students found in trash
	                    [parent_item_colon] => Parent Page
	                )
	 
	            [description] => My students
	            [show_ui] => 1
	            [public] => 1
	            [use_default_menu_icon] => 1
	            [label] => student
	            [cctm_hierarchical_includes_drafts] => 1
	            [cctm_hierarchical_post_types] => Array
	                (
	                    [0] => post
	                    [1] => page
	                )
	 
	            [menu_position] => 
	            [rewrite_with_front] => 1
	            [permalink_action] => Off
	            [rewrite_slug] => 
	            [query_var] => 
	            [capability_type] => post
	            [show_in_nav_menus] => 1
	            [can_export] => 1
	            [hierarchical] => 
	            [rewrite] => 
	            [is_active] => 1
                    // contains list of keys from custom_field_defs
	            [custom_fields] => Array( portrait, sex),
	            [map_field_metabox] => Array 
                          (
                             [portrait] => default,
                             [sex] => sidebar,
                          ),
	),
   // unique name for each field
   [custom_field_defs] => Array (
	    [portrait] => Array
	        (
	            [label] => Portrait
	            [name] => portrait
	            [default_value] => 
	            [description] => Mug shot
	            [output_filter] => to_image_tag
	            [type] => image
	        ),
	    [sex] => Array
	        (
	            [label] => Sex
	            [name] => sex
	            [default_value] => 
	            [extra] => 
	            [class] => 
	            [options] => Array
	                (
	                    [0] => Male
	                    [1] => Female
	                )
	
	            [description] => 
	            [type] => dropdown
	        ),
	),
   // unique name for each metabox
   [metabox_defs] => Array
        (
            [default] => Array
                 (
                     [id] => default,
                     [title] => Custom Fields,
                     [context] => normal,
                     [priority] => default,
                     [post_types] => Array ( post, page ),
                     [callback] => ,
                     [callback_args] => ,
                 ),
        ),
   [export_info] => Array
        (
            [title] => My Students
            [author] => Everett
            [url] => http://www.fireproofsocks.com/
            [description] => Example
            [template_url] => http://www.themeforrest.com/my-theme /*Where can I download a theme  that utilizes this definition? */
        )
   [cctm_version] => '0.9.3.3-dev'  // used to test if we need to run updating scripts
   [cctm_installation_timestamp] => 1303780421
   [cctm_update_timestamp] => 1303780421
   // The text of each warning is hashed to uniquely identify it
   [warnings] => Array(
      'There was some problem!' => 23415241, /* timestamp when it was viewed and cleared*/
      'There was some other problem' => 0, /* 0 = still active... not cleared yet*/
   )

   // We do a wee bit of caching here to avoid directory scans
   [cache] => Array (
      [elements] => Array(
         [dropdown] => '/full/path/to/dropdown.php',
         // ... all available custom field types ...
      ),
      [filters] => Array(
         [to_array] => '/full/path/to/to_array.php',
         // ... all available output filters ... 
      ),
   )
   // flash messages: keyed off user_id so we know which messages should be displayed for each user
   [flash] => Array (
    '123' => 'This is my flash message',
   ),
   // FUTURE: track pages and settings that are being edited
   [locks] => Array(
      'some_page' => user id
   ) 
)
```



Note that each node of the _cctm\_post\_type\_def_ array should be passable to the `register_post_type()` function below without any modification.  The keys that are not native to WordPress are prefaced with "cctm_", e.g._

  * **cctm\_hierarchical\_includes\_drafts**: will cause WordPress' parent selector to include unpublished pages
  * **cctm\_hierarchical\_post\_types**: this determines which post types are viable candidates for parents.



# register\_post\_type #

For the record, this demonstrates sample input for WordPress' `register_post_type()` function:

```
	$def = Array
	(
	    'supports' => Array
	        (
	            'title',
	            'editor'
	        ),

	    'post_type' => 'book',
	    'singular_label' => 'Book',
	    'label' => 'Books',
	    'description' => 'What I&#039;m reading',
	    'show_ui' => 1,
	    'capability_type' => 'post',
	    'public' => 1,
	    'menu_position' => '10',
	    'menu_icon' => '',
	    'show_in_nav_menus' => '',
	    'can_export' => '',
	    'is_active' => 1,
	);
```

# See Also #

[DefinitionStructure](DefinitionStructure.md) -- shows data structure used in the importing and outputting of `.cctm.json` definition files.