

# Description #

Use this configuration file to customize the search form that is visible when you are defining an image field and click the "Set Search Parameters" button.

# Customizing the Search Parameters #

|![https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg](https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg)|All CCTM customizations rely on the same principle: if you create your own configuration file it will override the built-in default configuration file.  Several areas of functionality can be controlled via this method. See [Customizations](Customizations.md) for more info.|
|:--------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

To customize the configuration for all relation fields, perform the following steps:

  1. Via FTP, navigate to your site's **wp-content/uploads** folder.  If there isn't already a folder named **cctm** there, create it.  NOTE: change this if you have customized the location of your uploads folder.
  1. Inside of the **cctm** folder, create a folder named "config" (it may already exist).
  1. Inside of the **config** folder, create a folder named "search\_parameters"
  1. Into the **search\_parameters** folder, copy the `_image.php` file from `wp-content/plugins/custom-content-type-manager/config/search_parameters/_image.php`.


The end goal here is to have a file inside your _own_ folder outside the CCTM's folder.  <font color='red'><b>DO NOT EDIT FILES INSIDE THE CCTM's FOLDER</b></font>.  Any plugin files may be deleted or updated during an upgrade, so those files are off-limits!

Update your copy of the `_image.php` file so that it contains the search criteria you need.  This will unlock new form elements when you go to define your field.  For example, if you want your image fields to be defined so that users may only select images that have been tagged as belonging to a certain category, you could add the "taxonomy" and "taxonomy\_term" as search\_by criteria:

```
CCTM::$search_by[] = 'post_mime_type';
CCTM::$search_by[] = 'search_term';
CCTM::$search_by[] = 'search_columns';
CCTM::$search_by[] = 'match_rule';
CCTM::$search_by[] = 'post_parent';
CCTM::$search_by[] = 'taxonomy';
CCTM::$search_by[] = 'taxonomy_term';
```




Note that the `_image.php` file to all image fields, etc.  You can use the _exact_ name of the custom field for your configuration file if you require one field to have a specific set of criteria, e.g. `config/search_parameters/myimage.php` would define the search form to be displayed for only the "myimage" field.