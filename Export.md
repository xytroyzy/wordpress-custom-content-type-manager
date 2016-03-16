Starting with version 0.8.9, you can [Import](Import.md) and Export your content type definitions.

# Information #

When you export your definitions, you are also required to provide some meta data about that definition.

  * **Title** : Give a short title to your download. _(required)_
  * **Author** : _(required)_
  * **URL** : this is your chance to let the world know your site!
  * **Template URL**: if there is a theme out there that utilizes your definition, let us know where we can download it.
  * **Description** : please give a brief description of your content types and custom fields _(required)_.  The description will help people know

## What Is Exported ##

All of your custom content  types and all of thecustom field definitions are included in the exported definition.  This includes menu labels, URL settings, and custom menu icons.

## What is Not Exported ##

The definitions you download only describe your post-types and your custom fields: they do not contain any of your site's content.

Due to how the internal references work, the default values for image-, media-, or relation-fields cannot be exported.  This because the default value is a _reference_ to a post ID on your site -- a foreign key.  There is no guarantee that the person who imports your definition will have that same post on his or her site.  So during the course of exporting your definition, the default values for those field types are zeroed out so that the definition can be "site agnostic" and be safely deployed on any site.

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | <font color='red'>NOTE</font>: Exporting CCTM definitions does NOT export your site's content!|
|:--------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------|


# Download to File #

Click the **Custom Content Type --> Tools --> Export** menu item in the WordPress manager, then click the "Download" button.

The resulting downloaded file will have a name based off of your **title**, and it will use the extension of `.cctm.json`.  This is a plain text file containing a [JSON](http://en.wikipedia.org/wiki/JSON) array of all of your definition data.

# Save to Library #

Rather than downloading a definition file, this option lets you store the definition file directly to your web server (inside of `wp-content/uploads/cctm/defs/`).  This is a useful way to save your work and come back to it later if needed.