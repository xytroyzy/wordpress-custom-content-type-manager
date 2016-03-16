Frequently Asked Questions:




---


## I want posts and pages to have this functionality ##

I've gotten several emails and forum posts about "adding" custom field functionality to posts and pages.  That functionality has always been part of CCTM.  You simply need to "Standardize" your post's or page's custom fields.

![http://content.screencast.com/users/fireproofsocks/folders/Snagit/media/f90575a0-72f6-4b9e-aac5-647574f21669/2013-03-05_11-07-10.png](http://content.screencast.com/users/fireproofsocks/folders/Snagit/media/f90575a0-72f6-4b9e-aac5-647574f21669/2013-03-05_11-07-10.png)

This is an option that you can configure: some sites and some plugins handle custom fields for posts and pages differently, so the CCTM does not take over the custom fields on posts and pages by default.


---


## My other plugin cannot load correctly! ##

If you are trying to activate the CCTM when you already have the Summarize Posts plugin active, you will get this error:

```
The Summarize Posts plugin cannot load correctly! Another plugin has declared conflicting class, 
function, or constant names: Class: uninstall_cctm You must deactivate the plugins that are using these 
conflicting names.
```

or the error reads in part:

```
The Custom Post Type Manager plugin cannot load correctly!
Another plugin has declared conflicting class, function, or constant names:
```

You must deactivate the Summarize Posts plugin.  _Everything_ from the Summarize Posts plugin is in the CCTM, so there is no reason to use both.  The error stems from a simple PHP reality: you cannot redeclare class or function names.


---


## The CCTM Menu Does not Display! ##

Some users report that the CCTM menu does not appear after installing the plugin.  This seems to occur more often when the site is running as a multi-site setup.  Configuring the admin menu code is done following the instructions here: [configuring the admin menu](Config_admin_menu.md).  Sometimes WordPress fails silently if another plugin is already using a menu space, and if you are using the MU setup, then you may need to adjust the permissions of the menu code.


---


## My Image/Relation/Media Field Only Shows Numbers. WTF?!? ##

Every time you use a relation, media, or image field, you don't store the path to the image, you store the _unique ID_ of that image or post (in database lingo, this is called a "foreign key").  This is one of the tenets of [database normalization](http://en.wikipedia.org/wiki/Database_normalization) (this is exactly how WordPress stores the featured image and any attached images, by the way).

The point here is you don't want the number, you want the image that the number _represents_.  The CCTM offers several [Output Filters](OutputFilters.md) to help you do something with that number easily, or you can use several built-in WordPress functions to translate the number into something useful. For example, in your template file:

```
<?php print_custom_field('my_relation:to_link'); ?>
OR
<?php print_custom_field('my_image:to_image_tag'); ?>
```

See the documentation and examples on the [Image](Image.md) page for more examples (or check the [Relation](Relation.md), [Media](Media.md) pages), or read up on [Output Filters](OutputFilters.md) to see how they can help you customize your output.

### Get images from a single field ###

Here's an example using the [to\_image\_tag](to_image_src_OutputFilter.md) filter:
```
<?php print_custom_field('my_image:to_image_tag'); ?>
```

If you want a bit more control, then you can use the [to\_image\_src](to_image_src_OutputFilter.md) filter:
```
<img src="<?php print_custom_field('my_image:to_image_src'); ?>" />
```

Just to be perfectly clear here, all we're doing with this is looking up the post data associated with the number.  If the image ID is 123, then you would get the image's source by doing this:

```
<?php print CCTM::filter(123, 'to_image_src'); ?>
```

Or if you want to do the same thing using just WordPress functions and leave CCTM out of it:

```
<?php
$image_id = 123;
$img = wp_get_attachment_image($image_id);
print $img;
?>
```

Make sense?  We're just converting a post ID (which happens to be an image post), and converting it to the data we want.

### Get images from a multi-image field ###

Things get a bit more complicated when you have multiple images stored in your field (i.e. an image field with the "repeatable" box checked).  Remember that when you have this box checked, you are storing an ARRAY of values: a list.  Not a single image, but a list of images.  And the CCTM stores this data as a JSON array. See the discussion on [data structure](CustomFieldsDataStructure.md) for all the gory technical details.

So let's get the array of image IDs and convert them.  The [to\_array](to_array_OutputFilter.md) filter here is critical to ensure that you're dealing with an array of images:

```
<?php
$img_ids_array = get_custom_field('my_images:to_array');
foreach ($img_ids_array as $img_id) {
   print CCTM::filter($img_id, 'to_image_tag');
}
?>
```

We can make this a bit cleaner by passing an argument to the 'to\_array' filter.  It accepts the name of another output filter as its argument:

```
<?php
$img_array = get_custom_field('my_images:to_array', 'to_image_tag');
foreach ($img_array as $img) {
   print $img;
}
?>
```


---

## I used a Multi-Relation field to select 3 other posts.  How do print links to them with title, thumbnail, etc? ##

The trick here is that the relation fields store only the post IDs of those related posts, and you don't want the post's ID, you want the post that the ID represents.  In such a case, you'll find the [get\_post](get_post_OutputFilter.md) Output Filter extremely helpful.  Instead of just retrieving the link to the other post (like the [to\_link\_href](to_link_href_OutputFilter.md) or [to\_link](to_link_OutputFilter.md) filters do), the [get\_post](get_post_OutputFilter.md) gets _everything_ from the referenced post, leveraging the power of the [GetPostsQuery::get\_post](http://code.google.com/p/wordpress-summarize-posts/wiki/get_posts) function from the integrated [Summarize Posts](http://code.google.com/p/wordpress-summarize-posts/) plugin.

What does that mean?  It means you can easily get the post's title, date, and _all_ of its custom fields.

#### If it's a Repeatable Field ####

If your custom field is a "repeatable" field that allows you to store multiple values, then you must first use the [to\_array](to_array_OutputFilter.md) filter to ensure that you retrieve an array of values.  You can pass the name of a 2nd Output Filter and each member of the array will get filtered by this secondary filter.

```
$my_posts = get_custom_field('select_relative_posts:to_array', 'get_post');
foreach ($my_posts as $p) {
    print $p['post_title'];
    print $p['guid'];  // this is the permalink
    print $p['my_custom_field']; // any defined custom field is available
    print $p['thumbnail_src'];  // This is the src of the "Featured Image", if set
    print_r($p); // this will show you EVERYTHING that's available in that post
}
```

Once we've got an array of post IDs, then all you have to do is come up with some way to look up the data represented by that ID.  You can use WordPress' own functions for this (e.g. [get\_permalink()](http://codex.wordpress.org/Function_Reference/get_permalink) or [get\_post()](http://codex.wordpress.org/Function_Reference/get_post), but the CCTM's get\_post() function has the advantage of automatically retrieving all the custom fields, whereas with WordPress' built-in functions, you'd have to use 2 separate functions: one to get the post, and another to get the post's custom fields.

Note that if you are trying to get the "Featured Image", !Wordpress stores that in a hidden custom field named `_thumbnail_id`; GetPostsQuery automatically looks up the "thumbnail\_src" attribute for any featured image set.  See that?  WordPress also stores only the ID of the image.

#### If it's a singular field ####

The simple non-repeatable version of the relation field is easy to work with.  You simply need a way to convert a post ID into something useful.

```
<?php
$my_post = get_custom_field('select_relative_posts:get_post');

print $my_post['post_title'];
print $my_post['guid'];  // this is the post's permalink
print $my_post['my_custom_field'];  // any defined custom fields are also retrieved

print_r($my_post); // this will show you all the data available for that post, custom fields and all
?>

Or more a more practical example:

<a href="<?php print $my_post['guid']; ?>"><?php print $my_post['post_title']; ?></a>
```

#### If it's an image field ####

What's important here is that the post's "guid" attribute is where the image src is stored.

```
<?php
$my_img = get_custom_field('related_image:get_post');
?>

<img src="<?php print $my_img['guid'];?>" alt="<?php $my_img['post_title']; ?>" />
```



---


## How do I enable "friendly" URLs (a.k.a. "Pretty Permalinks") for my post-type? ##

The default behavior of WordPress is to support URLs like  http://site.com/?post_type=book&p=39.  You can change the URL format of your _posts_ by changing the permalink settings for your blog (see http://codex.wordpress.org/Using_Permalinks).  However, these settings are not global: they do not apply to your custom post-type.  You _do_ need to have the ability to rewrite URLs on your server, but merely enabling custom permalinks under **Settings --> Permalinks** will have no immediate effect on your custom post-types.  Here's how to set your custom post-types to use a friendly URL structure:

1. When you define your post-type in the CCTM, be sure to visit its "URLs" tab and enable the "Permalink Action" dropdown to either `/%postname%/` or to `Custom`.  If you're using the custom option, you can enter your own settings in the "Rewrite Slug".

2. Go to the WordPress **Settings --> Permalinks** page, and change the permalinks settings (e.g. back to default) and save them.

3. While still under **Settings --> Permalinks**, change the format back to how you want them and save the settings again.  It seems that it is the act of _changing_ the global permalink settings which causes WordPress to integrate the URL/permalink settings for your custom post-types.


WordPress' support of custom permalink structure is often limited and difficult to understand, so your success here may vary.  For a much cleaner alternative, try the [MODX CMS](http://modx.com/).



---


## What does Activating a Post-Type do? ##

"Activating" a built-in post-type (i.e. pages or posts) will force their custom fields to be standardized.  This means that the default WordPress metabox for custom fields is overridden by the CCTM, so instead of adding multiple text custom fields wily-nilly, you will need to define them and associate them with your post-type using the CCTM.

Activating a post-type ensures that it gets registered with WordPress. Once the content type is registered, a menu item will get created (so long as you checked the "Show Admin User Interface" box) and you ensure that its custom fields become standardized. If the "Public" box was checked for this content type, then the general public can access posts created under this content type using the URL structure defined by the "Permalink Action" and "Query Var" settings, e.g. http://site.com/?post_type=book&p=39





---


## My custom fields do not show up on my post or page! ##

You have to _Standardize_ your custom fields in order for them to show up on your posts and pages in the manager.  Click the Custom Content Types menu and be sure that you have standardized the custom fields for posts or pages.  For the front-end of the site, you need to modify your template files to print the extra data.  See [SampleTemplates](SampleTemplates.md) for more info.

Why is it called "standardizing" custom fields?

When you standardize the fields of a post-type, you override the default WordPress metabox for custom fields, replacing it with the CCTM's custom fields metabox.  The standard WordPress behavior is to allow any number or combination of fields on _any_ post -- i.e. they are not standardized at all.  So when you standardize the fields it forces each post or page to have the exact same fields on each post to the custom fields you have defined for that post-type.

You can standardize the custom fields for posts, pages, or any 3rd-party post-type (as of 0.9.5.11).  If you wish to customize the fields for a foreign post-type (i.e. a post-type registered by some other plugin), just ensure that you have updated the CCTM global settings to display foreign post-types.

Custom post-types will have their fields standardized automatically, but for posts and pages (i.e. the built-in post-types), you can choose whether you want to enforce this behavior or not.


---


## What types of custom fields are supported? ##

See the list of [Supported Custom Fields](SupportedCustomFields.md).  You can also define your own [Custom Custom Fields types](CustomCustomFields.md).


---


## How do I add images or video into a custom field? ##

The media-related custom fields tie into WordPress' "attachment" posts, so if you have already uploaded images or video using the Media menu, they will show up for selection when edit a post using a custom image or media field.  You can also choose "Add New Image" when you browse existing images.


---


## How do I make my custom field values show up in my templates? ##

Content and templates must go hand in hand. If you have defined custom fields, you have to modify your theme files to accommodate them.  There are two included theme functions intended to help you with this task:

  * [get\_custom\_field()](get_custom_field.md) -- gets the value
  * [print\_custom\_field()](print_custom_field.md) -- prints the value

In this plugin's settings area, each content-type has a link to "View Sample Templates" -- this page gives you a fully customized example showing demonstrating a custom theme file for your custom content type.

See the wiki page on [TemplateFunctions](TemplateFunctions.md)


---


## When I print my custom field in my template, I only see "Array"! ##

If you see "Array" instead of the value you stored in your field, then you are actually storing a list of values (i.e. an array).  This happens if you are using a multi-select field or if you've checked the "Repeatable" option on your field definition.  If you are storing a list of values, then you can't simply print it, you have to iterate over the items in the list.  You can use the [formatted\_list](formatted_list_OutputFilter.md) Output Filter to help print a list from the array, or you can manually iterate over the items in the list -- see an example of this in the [to\_array](to_array_OutputFilter.md) Output Filter.


---


## Everything Broke after Moving my Site! ##

This is usually due to a WordPress error: WordPress hardcodes paths and URLs when it stores options as PHP serialized data, which includes a character count.    So if you change your domain name, for example, the character count changes, and the serialized data structure becomes corrupted.  In general, WordPress is very poorly suited to site migration in part due to this reason.

**What to do?**

  1. On the site you are moving, make a full backup of the files and database.
  1. On the site you are moving, go to the CCTM's **Tools** tab and [Export](Export.md) your custom field definitions to a .cctm.json file.  Save this file!
  1. Migrate your WordPress site as per usual, moving both the files and the database to the new location.
  1. In the new site, log into the WordPress manager and head back to the CCTM's **Tools** tab and [Import](Import.md) the definition file you created back on the old site.  Because the settings are stored as a JSON object, it is not vulnerable to the limitations of the serialized data.



---


## How Do I Add a Translation? ##

See the [Translations](Translations.md) page for information.