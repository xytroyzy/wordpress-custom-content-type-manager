Tools offer additional functionality for the CCTM plugin.

# Available Tools #

## Import ##

This allows you [Import](Import.md) a CCTM definition file.  The definition files can be created by any other site running 0.9.4 or later of the CCTM.  The files are in the JSON format and use a `.cctm.json` extension.

## Export ##

Use the export tool to [Export](Export.md) a CCTM definition file.  You can either save to your local library or download a copy of the definition file.

## Clear Cache ##

The CCTM caches certain things for faster performance.  This data includes:

  * Any warnings that the CCTM has displayed
  * The paths to formatting templates (tpls)
  * Thumbnail images generated for use in the manager

Clearing the cache is necessary when you're [customizing the manager HTML](CustomizingManagerHTML.md).

## Purge Posts ##

**BE CAREFUL!** Using this tool can permanently delete data from your database.  Relying on custom post-types can end up filling your database with lots of data.  The problem comes when a plugin no longer registers the post-type with WordPress, so WordPress "forgets" about the post-type, but the database does not.  If you've been experimenting with multiple plugins and you have created lots of posts, then this tool can help you weed out unwanted rows from your database.  This will remove posts that belong to any non-registered post-type.