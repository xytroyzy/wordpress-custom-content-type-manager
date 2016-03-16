

# Description #

Use this configuration file to customize the behavior of the search forms in the modal-windows that pop up when you click a "Choose Media" button.


# Customizing the Search Parameters #

|![https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg](https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg)|All CCTM customizations rely on the same principle: if you create your own configuration file it will override the built-in default configuration file.  Several areas of functionality can be controlled via this method. See [Customizations](Customizations.md) for more info.|
|:--------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

To customize the configuration for all relation fields, perform the following steps:

  1. Via FTP, navigate to your site's **wp-content/uploads** folder.  If there isn't already a folder named **cctm** there, create it.  NOTE: change this if you have customized the location of your uploads folder.
  1. Inside of the **cctm** folder, create a folder named "config" (it may already exist).
  1. Inside of the **config** folder, create a folder named "post\_selector"
  1. Into the **post\_selector** folder, copy the _**media.php** file from `wp-content/plugins/custom-content-type-manager/config/post_selector/_media.php`._

The end goal here is to have a file inside your _own_ folder outside the CCTM's folder.  <font color='red'><b>DO NOT EDIT FILES INSIDE THE CCTM's FOLDER</b></font>.  Any plugin files may be deleted or updated during an upgrade, so those files are off-limits!

Update your copy of the `_media.php` file so that it contains the search criteria you need.  For example, if you want the search function to only search for post\_titles (instead of searching titles and content), update the file to contain a line like the following:

```
CCTM::$post_selector['search_columns'] = array('post_title');
CCTM::$post_selector['post_type'] = 'attachment';
CCTM::$post_selector['post_mime_type'] = '';
CCTM::$post_selector['post_status'] = array('publish','inherit');
CCTM::$post_selector['orderby'] = 'ID';
CCTM::$post_selector['order'] = 'DESC';
CCTM::$post_selector['limit'] = 10;
CCTM::$post_selector['paginate'] = 1;

CCTM::$search_by = array();
CCTM::$search_by[] = 'post_type';
CCTM::$search_by[] = 'search_term';
CCTM::$search_by[] = 'post_status';
CCTM::$search_by[] = 'taxonomy';
CCTM::$search_by[] = 'taxonomy_term';
CCTM::$search_by[] = 'post_parent';
CCTM::$search_by[] = 'meta_key';
CCTM::$search_by[] = 'meta_value';
```