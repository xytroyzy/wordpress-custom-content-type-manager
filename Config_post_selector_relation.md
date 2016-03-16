

# Description #

Use this configuration file to customize the behavior of the search forms in the modal-windows that pop up when you click a "Choose Relation" button.

# Customizing the Search Parameters #

|![https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg](https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg)|All CCTM customizations rely on the same principle: if you create your own configuration file it will override the built-in default configuration file.  Several areas of functionality can be controlled via this method. See [Customizations](Customizations.md) for more info.|
|:--------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

To customize the configuration for all relation fields, perform the following steps:

  1. Via FTP, navigate to your site's **wp-content/uploads** folder.  If there isn't already a folder named **cctm** there, create it.  NOTE: if you have customized the location of your uploads folder, you'll have to navigate to that custom directory.
  1. Inside of the **cctm** folder, create a folder named "config" (it may already exist).
  1. Inside of the **config** folder, create a folder named "post\_selector"
  1. Into the **post\_selector** folder, copy the `_relation.php` file from `wp-content/plugins/custom-content-type-manager/config/post_selector/_relation.php`.


The end goal here is to have a file inside your _own_ folder outside the CCTM's folder.  <font color='red'><b>DO NOT EDIT FILES INSIDE THE CCTM's FOLDER</b></font>.  Any plugin files may be deleted or updated during an upgrade, so those files are off-limits!

Update your copy of the `_relation.php` file so that it contains the search criteria you need.  For example, if you want the search function to only search for post\_titles (instead of searching titles and content), update the file to contain a line like the following:

```
CCTM::$post_selector['search_columns'] = array('post_title');
CCTM::$post_selector['post_status'] = array('publish','inherit');
CCTM::$post_selector['orderby'] = 'ID';
CCTM::$post_selector['order'] = 'DESC';
CCTM::$post_selector['limit'] = 10;
CCTM::$post_selector['paginate'] = 1;

CCTM::$search_by = array();
CCTM::$search_by[] = 'post_type';
CCTM::$search_by[] = 'post_status';
CCTM::$search_by[] = 'taxonomy';
CCTM::$search_by[] = 'taxonomy_term';
CCTM::$search_by[] = 'post_parent';
CCTM::$search_by[] = 'meta_key';
CCTM::$search_by[] = 'meta_value';
```

Other options could include any built-in WordPress column or any custom column, e.g. post\_author, my\_custom\_field, post\_status etc.

See the documentation for http://code.google.com/p/wordpress-summarize-posts/wiki/get_posts to see other valid array keys.

Note that the `_relation.php` file applies to all relation fields, the `_image.php` file to all image fields, etc.  You can use the _exact_ name of the custom field for your configuration file if you require one field to have a specific set of criteria, e.g. `config/post_selector/myrelation.php` would define the search form to be displayed for the "myrelation" field.