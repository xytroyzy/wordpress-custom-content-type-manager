# Settings #

Starting with version 0.9.4 of the plugin, you can adjust various settings that affect how the plugin will behave.



<a href='http://www.youtube.com/watch?feature=player_embedded&v=wdsLgy0ocpo' target='_blank'><img src='http://img.youtube.com/vi/wdsLgy0ocpo/0.jpg' width='425' height=344 /></a>

## Delete Posts ##

If this option is checked, then [deleting a content type definition](DeletePostType.md) will actually delete any posts of that type from your database!  If you delete the posts from the database, it cannot be undone! So please be careful if you choose to use this option!


---


## Delete Custom Fields ##

If this option is checked, then deleting a field definition will cause any instances of that field to be deleted from the database (from the **wp\_postmeta** table).  This cannot be undone!  so be careful!


---


## Show Custom Fields Menu ##

It cane be troublesome to have to navigate to the CCTM's admin menu option each time you want to make a change to the custom fields for each post-type.  Check this option if you want a dedicated "Custom Fields" link to appear in the menu for each content type.

![https://img.skitch.com/20110930-b5km1s23uhdc373tb8pxm64ynp.png](https://img.skitch.com/20110930-b5km1s23uhdc373tb8pxm64ynp.png)


---


## Show Settings Menu ##

It cane be troublesome to have to navigate to the CCTM's admin menu option each time you want to make a change in your post-type definition.  Check this option if you want a dedicated "Settings" link to appear in the menu for each content type.

![https://img.skitch.com/20110930-nfjbu966xp458x53ya5x5yjkqs.png](https://img.skitch.com/20110930-nfjbu966xp458x53ya5x5yjkqs.png)


---

## Display Foreign Post Types ##

The CCTM is not the only plugin that might be using custom post-types on your site.  Optionally, you can choose to display the foreign post-types in your CCTM manager screens.   Seeing the other post-types can help increase your awareness of what other plugins are doing on your site, or maybe you're just curious.  Enabling this option is merely a helpful "FYI" insight into your site.

![https://img.skitch.com/20110930-bie5w6w563ysxd7im5bk9ryeu1.png](https://img.skitch.com/20110930-bie5w6w563ysxd7im5bk9ryeu1.png)


---

## Cache Thumbnail Images ##

When using image, media, or relation fields, a small thumbnail image is displayed to users when they sort through the available posts to select a value for the field.  WordPress usually uses the _full sized image_ for these "thumbnails", which can make for some very slow page-loads.  You can make CCTM generate small, low-quality images for this purpose, but note that this feature is currently experimental: some servers will quietly fail if the images they are operating on are too large.  Your post-selector screens will simply go white.  The planned workaround is to resize the images via ImageMagik, which stores the data on disk as it manipulates it, so you don't run into the memory issues that is inherent with vanilla PHP resizing, but ImageMagik is not available on every server.


---

## Save Empty Fields ##

When you save a post with custom fields, you have an option here to control whether or not you wish to create a row in **wp\_postmeta** for empty field values.  If your database space is at a premium, uncheck this so you don't create unused rows.  Some reporting and searching queries may be easier to construct if you do have an empty field in place as a kind of placeholder (e.g. JOIN statements may be more easily constructed), so the default option here is checked: empty fields will contain their own row in the **wp\_postmeta** table.


---

## Summarize Posts TinyMCE Button ##

With the integration of the Summzarize Posts plugin into the CCTM, the WYSIWYG editors will display a small button that assists you in creating valid Summarize Posts shortcodes.  If you would prefer for this button to _not_ be displayed, uncheck this option.

![https://img.skitch.com/20120203-1n9tjx4q4j13utnps9ct5r28up.jpg](https://img.skitch.com/20120203-1n9tjx4q4j13utnps9ct5r28up.jpg)


---

## Flush Permalink Rules ##

When registering custom post-types, you usually need the CCTM to flush the WordPress permalink rules in order to support URL mappings to your custom posts.  Other plugins may also perform this same operation, so you can save on the overhead by disabling this: flushing permalink rules is an expensive operation, so repeating it needlessly can cause your site's performance to suffer.



---

## Show Pages in RSS Feed ##

Sometimes you want more than just your _posts_ to show up in your RSS feed.  Check this option to make your _pages_ show up in the feed.  If you want other custom post-types to show up in the feed, see the individual settings for each post-type.


---

## Right Now Widget Support ##

In the default WordPress dashboard, there is a manager widget called "Right Now".  It shows the latest posts and pages.  When you define each custom post-type, you can choose whether or not you want it to show up in this widget, or you can turn off support for custom post-types altogether by unchecking this option.


---


# Custom Field Settings #

Some custom fields may require their own settings pages (e.g. for entering an API key).  3rd party developers can create their own custom fields (instructions on that are coming), but depending on the requirements of the field, there may be settings options that appear for each type of field.