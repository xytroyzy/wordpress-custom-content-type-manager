

# Description #

You can customize the CCTM's admin menu (pictured) via configuration file.

![https://img.skitch.com/20120201-qkwh8r23d44huk1ngwys13erx9.jpg](https://img.skitch.com/20120201-qkwh8r23d44huk1ngwys13erx9.jpg)

# Customizing the Admin Menu #

|![https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg](https://img.skitch.com/20120201-r687d2fxjkdfg6feua52t6wwc8.jpg)|All CCTM customizations rely on the same principle: if you create your own configuration file it will override the built-in default configuration file.  Several areas of functionality can be controlled via this method. See [Customizations](Customizations.md) for more info.|
|:--------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

  1. Copy the `wp-content/plugins/custom-content-type-manager/config/menus/admin_menu.php` file to `wp-content/uploads/cctm/config/menus/admin_menu.php`
  1. Edit your copy to taste.


## Changing Menu Permissions ##

A common thing to edit is the permissions of the menu.  This is especially important for a multi-site installation.  For example, to make the CCTM menu available for each site in a multi-site install, you can comment out the following:

```
/*
if (defined('WP_ALLOW_MULTISITE') && WP_ALLOW_MULTISITE == true && is_super_admin()) {
	$capability = 'manage_network';
}
*/
```

## Changing Menu Position ##

Another common customization is to change where the menu gets drawn.  WordPress can experience conflicts if two plugins try to create a menu item in the same place, so sometimes you have to edit this "menu position" number.

If you need to change where the menu gets drawn, use your own number in place of the **self::menu\_position** class constant.  For example, replace the class constant with _77_:

```
// Main menu item
add_menu_page(
	__('Manage Custom Content Types', CCTM_TXTDOMAIN),  // page title
	__('Custom Content Types', CCTM_TXTDOMAIN),      // menu title
	$capability,						// capability
	'cctm',								// menu-slug (should be unique)
	'CCTM::page_main_controller',       // callback function
	CCTM_URL .'/images/gear.png',       // Icon
	77					// menu position
);
```

## Changing the Menu Text ##

If your layout is cramped, you can easily change the text used for the menu.  In the example below, we've replaced the 'Custom Content Types' menu text with a shorter 'CCTM' label:

```
// Main menu item
add_menu_page(
	__('Manage Custom Content Types', CCTM_TXTDOMAIN),  // page title
	'CCTM',      // menu title
	$capability,						// capability
	'cctm',								// menu-slug (should be unique)
	'CCTM::page_main_controller',       // callback function
	CCTM_URL .'/images/gear.png',       // Icon
	self::menu_position					// menu position
);
```