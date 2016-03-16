# 0.8.9 #

  * Permalink functionality fixed by flushing rewrite rules after register\_post\_type()
  * Flash messages are now stored in the $_COOKIE array instead of $_SESSION to be in keeping with WordPress' simplistic "stateless" parlance.
  * Date field added, including support for PHP eval of default value.
  * CSS classes and ids updated (all instances of "formgenerator" were replaced with "cctm").
  * Import/Export functionality added.
  * Checking of valid image icons was disabled due to problems with segfault in some server configurations [see Issue 60](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=60)
  * Default values for dropdowns now settable via Javascript to reduce typos
  * Bug with page-attributes fixed: you can now correctly enable page attributes for custom content types.
  * Support for Post Formats added.
  * Support for [Output Filters](OutputFilters.md) was added.
  * Template functions `get_custom_image()` and `print_custom_image` were removed in favor of using Output Filters.
  * The FormGenerator class removed in favor of simpler PHP pages.

# 0.8.8 #

  * More CRUD-like interface for creating and editing custom fields one at a time.
  * Object-Oriented class structure implemented for custom fields in accordance with future plans for more field types
  * Drag and Drop interface added for Custom Fields to change sort order
  * Added support for built-in taxonomies (Categories and Tags).
  * Fixed unreported bugs affecting custom tpls.
  * Fixed bug causing popup thickbox to load incorrectly: [Issue 17](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=17&can=1)
  * Styling for the manager updated to match what's used by WordPress 3.1.
  * Greatly improved administration interface including support for icons and a series of tabs for dividing the multi-part form for creating/editing content types.
  * Reduced MySQL requirements to version 4.1.2 (same as WordPress 3.1) after feedback that the plugin is working fine in MySQL 4: [issue 28](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=28&can=1)
  * Fixed typos in CCTMTests.php re the CCTM\_TXTDOMAIN: [issue 29](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=29&can=1).
  * Added optional prefix for template **function get\_all\_fields\_of\_type()**: [Issue 26](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=26&can=1)
  * Three new template functions added per [Issue 25](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=25&can=1)
  * **get\_custom\_field\_meta()**
  * **print\_custom\_field\_meta()**
  * **get\_custom\_field\_def()**
See [Template Functions](TemplateFunctions.md) in the wiki.


# 0.8.7 (bad release: Jacked by WP repo) #
  * Adds HTML head and body tags back to the tpls/post\_selector/main.tpl to correct [issue 17](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=17&can=1).

# 0.8.6 #
  * Fixes bad CSS declaration: [issue 1](http://code.google.com/p/wordpress-custom-content-type-manager/issues/detail?id=1)
  * Fixed omission in sample template placeholders.

# 0.8.5 #
  * Resubmitting to placate WP's repository.

# 0.8.4 #

  * Resubmitting to placate WP's repository.

# 0.8.3 #

  * Customized manager templates. This is useful if you need to customize the manager interface for the custom fields.
  * Updated sample templates under the "View Sample Template" link for each content type.
  * Added image/media uploading functionality directly from the custom fields.
  * Fixed glitch in Javascript element counter that screwed up dropdown menu creation: due to this bug, you could only add a dropbox to the first element because the wrapper div's id was fixed statically as the same id, so adding a dropdown menu always wrote the new HTML to that same div (inside the first custom field).
  * Control buttons now at top and bottom of manage custom fields.
  * Links to bug tracking / wiki for this project.
  * Some basic HTML cleanup.
  * Moved some files around for better organization.

# 0.8.2 #
  * WordPress 3.0.4 fixed some bugs that affected the functionality of this plugin: now you CAN add custom content posts to WordPress menus.
  * WordPress has not recognized the updates to this plugin (apparently due to a glitch), so currently the only way to get the most recent version of this is to check it out via SVN.

# 0.8.1 #
  * Fixes problem saving posts. The problem had to do with wp-nonces and the admin referrer was being checked, but not generated, so the check failed. Oops.

# 0.8.0 #
  * Initial public release.