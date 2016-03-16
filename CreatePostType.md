Creating a new Content Type (known officially by WordPress as [post-types](http://codex.wordpress.org/Post_Types)) gives your WordPress site CMS functionality.  The CCTM plugin allows you to create custom content types (a.k.a custom post-types) by tying into WordPress' [register\_post\_type()](http://codex.wordpress.org/Function_Reference/register_post_type) function.

This page attempts to explain what some of those myriad settings actually do.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=YuP8sMV4yt4' target='_blank'><img src='http://img.youtube.com/vi/YuP8sMV4yt4/0.jpg' width='425' height=344 /></a>




---

# Basic #


## Post Type ##

The simplified name used in the database and URLs -- no special characters here.

## Menu Name ##

Specify the human-readable name for the menus.

## Description ##

Tell us a bit about this thing you're creating.  This will be shown if you share your definition via the Export tool.

## Use Default Menu Icon ##

With version 0.9.4 and later, you got a wide variety of custom icons to use.  If you use the built-in set, you'll get both the small menu icon AND a larger 32x32 icon that appears when you create or edit your pages.


---


# Labels #

I really tried to make this one as self-explanatory as possible: see the little "?" icons by each item so you can see where each label appears.


---


# Fields #

## Title ##

USE THIS.  WordPress gets really weird if you omit the post title.  Seriously, you need a name for every page, even if you don't want to show it on your final template, things are so much easier if you use the title.

## Content ##

You should **probably** use this too.  Certain things (notably the [WYSIWYG](WYSIWYG.md) fields) get really unruly if you don't have this field enabled.  Certain CSS and Javascript is tied to the main content block, so deactivating it can cause some unpredictable behavior.

## Author ##

Tracks the post author.

## Excerpt ##

A small summary field, good when you're generating menus or RSS feeds.

## Supports Custom Fields ##

This one is currently ignored by the CCTM because the CCTM plugin will _override_ the default behavior in order to customize and standardize the custom fields used.

## Post Formats ##

Some themes will take advantage of this feature... some don't.  Anything you do with Post Formats can be done without them (albeit with some more work in your templates), so this is merely a matter of convenience in design and workflow.

## Page Attributes ##

This opens up a metabox of various page attributes.

### Thumbnail ###

Give your post a "Featured Image".

### Hierarchical ###

Allows you to specify a parent page and build a hierarchy.

### Use Custom Hierarchy ###

This is where WordPress starts breaking down: it's structured as a _blog_, not as a CMS.  So if you start putting posts of one type as children of another post-type, WordPress starts getting weird, and the built-in tools don't help you see what's going on.  All of the built-in menus in WordPress are designed to show posts of a _single type_.  The custom hierarchy lets you break out of that mold, but once you leave the trail, you have a bit more work to do in regards to navigation.  Use carefully!


---


# Menu #

## Show in Menu ##

Most often this will be a simple "Yes", but starting with version 0.9.4, you can specify a custom top-level menu.  This lets you get a bit more creative in where your administration pages are placed, but you may need to adjust your CCTM [Settings](Settings.md) for this to work in a sensible manner.

## Menu Position ##

This is a basic sorting parameter.



---


# URLs #

This area is still in development, partly due to WordPress' inflexible and immature way of handling permalinks.

## Rewrite with Permalink Front ##

## Permalink Action ##

  * Off
  * /%postname%/
  * Custom

## Rewrite Slug ##

## Query Variable ##


---


# Advanced #

## Public ##

The public option here isn't specifically supported: we just set the defaults of the various options it represents.  "Public" here means that the following options are checked:

  * Show Admin User Interface
  * Show in Nav Menus
  * Publicly Queriable
  * Include in Search
  * Include in RSS feed

Normally these options are set together, but feel free to customize.

## Show Admin User Interface ##

I have a _really_ hard time imagining a case where you wouldn't check this: unless you're doing some serious mad-scientist stuff, this will be checked.


## Show in Nav Menus ##

If you want to put your posts form this type into WordPress' menus under **Appearance --> Menus**, then check this.  If you don't, you won't be able to select your posts to build your menu.

## Publicly Queriable ##

If you want your posts visible on the front-end via a URL, you'll check this.

## Include in Search ##

Use this to dictate whether or not your posts in this type will show up in search results.

## Include in RSS Feed ##

Use this to determine whether your posts in this type will show up in your RSS feed.

## Capability Type ##

This ties into WordPress permissions... currently, this hasn't been tested much.


## Can Export ##

Honestly, I've never used this feature...

## Trackbacks ##

Use this to enable the cross-blog [trackback](http://codex.wordpress.org/Introduction_to_Blogging#Trackbacks) feature.

## Enable Comments ##

If you check this, make sure your templates use the `comments_template();` code.

## Store Revisions ##

Revisions require more storage space in the database, but you can easily roll back your post to a previous draft.  <font color='red'>WordPress will not store custom fields in its revisions.</font>

## Enable Archives ##

This allows posts to be summarized using WordPress' built-in archive grouping functions.

## Show in Right Now Widget ##

WordPress includes a manager widget to show the latest posts. Check this box if you want your custom post-type to show up in this report.

## Order By ##

Choose a column to sort your manager view by.  This ONLY affects the list view in the manager, e.g. showing all pages, or showing all movies.

## Sort Order ##

Choose which direction you want to sort by: ascending or descending.

## Taxonomies ##

Use these features to make your site and posts more easily searchable.


  * Enable Categories

  * Enable Tags