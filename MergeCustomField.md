# Merge Custom Field Definitions #

Starting with version 0.9.4, you can merge custom field definitions.  To get to this function, in the WordPress manager, go to the **Custom Content Types** menu, and then select the **Custom Fields** option.  For each defined custom field, there is an option for **Merge**.

Merging causes a field to be renamed in the database and the old definition to be deleted. For example, if you merge "cats" into "dogs", all instances of "cats" in the database will be renamed to "dogs" and the field definition for "cats" will be removed.

The associated post\_types will be pooled when you merge field definitions.  For example, if the "cats" field was associated with the "post" content type and "dogs" was associated with the "page" content type, then merging "cats" into "dogs" will make "dogs" be associated with _both_ "post" and "page" content types.

Be careful about merging a field into a field of a different type! You may encounter unpredictable results!

# Details #

Add your content here.  Format your content with:
  * Text in **bold** or _italic_
  * Headings, paragraphs, and lists
  * Automatic links to other wiki pages