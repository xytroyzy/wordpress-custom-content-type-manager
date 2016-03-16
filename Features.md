# Features #

The Custom Content Type Manager (CCTM) is a WordPress that allows users to create custom content types (a.k.a. post\_types) with virtually any type of custom fields.  The CCTM makes WordPress into a true content management system (CMS).


## All types of Custom Fields ##

By default, WordPress only supports custom _text_ fields.  That's kind of boring, and it may not help you adequately display your content.  The CCTM offers [Date](Date.md) fields, [WYSIWYG](WYSIWYG.md) rich text fields, [Image](Image.md) fields, [Dropdown](Dropdown.md) fields and [more](SupportedCustomFields.md)!

## Repeatable Fields ##

Starting with version 0.9.5, you can select almost every field as "repeatable".  That means for one field, you can store an array of multiple values, e.g. multiple images or posts.

## Standardized Custom Fields ##

One huge shortcoming of WordPress is its inability to _standardize_ its custom fields.  If each of your posts requires a dozen custom fields, then each time you created a new post, you have to manually add those same 12 fields, over and over again.  Let's hope you don't mistype something!!

The CCTM plugin solves that problem by _standardizing_ your custom fields, so instantly, the same fields display in the same order for all your posts or pages!  No more guessing about which custom fields were used on your posts or pages!

## Sample templates ##

Every time you create custom content, you need custom templates to help you _display_ that data.  The CCTM dynamically generates [sample template code](SampleTemplates.md) for you -- you just need to copy and paste it into your theme.  This gives you a good head start when skinning all the various types of pages that your custom content types use.

## Widgets ##

The CCTM includes powerful widgets:

  * [Summarize Posts Widget](Widget.md) -- lets you display a list of nearly any posts
  * Post Content Widget -- shows the content of another post.

## Import and Export Definitions ##

If you are setting up several similar sites and you want them all to share the same content-types and custom fields, all you have to do is set up one site the way you want, then [Export](Export.md) the definition for use on the other sites.  This feature can save you lots of time and guarantee consistency across sites!