Starting with version 0.8.9, you can Import and [Export](Export.md) your content type definitions.  Note however that the format of the definitions changed with version 0.9.4!  Unfortunately, you cannot load up pre-0.9.4 definition files into CCTM versions 0.9.4 or later.

# Information and Warning #

Importing a definition can quickly and easily include multiple content types and custom fields on your site, so it can give you a great head start for rolling out a new site.  It provides a template for all aspects of your site _except content_.

It will also completely overwrite any existing definitions you may have created inside the Custom Content Type Manager!  Make sure you [Export](Export.md) your current definition before importing a new one if you think you might need to restore any of your existing definition data.

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | Importing a new definition  will overwrite the active definition! [Export](Export.md) a backup first! |
|:--------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------|

## What's In a CCTM Definition? ##

A CCTM definition file contains a lot of information, including:

  * All custom field definitions for the built-in post and page post-types
  * Custom content type definitions (e.g. books, movies, products)
  * All permalink preferences for custom content types
  * All custom field definitions for the custom content types
  * All menu preferences and labels

A CCTM definition contains _no post content of any kind_.


# Import from File #

CCTM definition files are stored as plain text JSON files, and they use the `.cctm.json` extension.

Click the "Custom Content Type" --> "Import" menu item in the WordPress manager, then click the "Upload" button.

The selected file must be a compatible `.cctm.json` file that was produced via the [Export](Export.md) feature.

See the page on [Definition Structure](DefinitionStructure.md) for a technical note on the data structure used by the import/export feature.