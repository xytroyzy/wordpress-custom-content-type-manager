# Available Languages #

The CCTM is available in the following translations:

  * English
  * German: Dr. Stahl
  * Romanian: Web Geek Sciense [Web Hosting Geeks](http://webhostinggeeks.com/)


# Making a Translation #

We would love to see the CCTM available in your language!  If you are able to provide a translation, refer to the **lang/custom-content-type-mgr.pot** file.  Using [PoEdit](http://www.poedit.net/) or a similar editor, create .po and .mo translations of the original .pot file.  See [Translating WordPress](http://codex.wordpress.org/Translating_WordPress) for more information, but here's a quick overview:

  1. Open the `lang/custom-content-type-mgr.pot` file in PoEdit
  1. Replace the translations in the right-hand column with the new translations.  If the language strings contain `%s`, these are placeholders for dynamic data, e.g. a fieldname used in the message.  Sometimes the language strings contain two or more placeholders, e.g. `%1$s` and `%2$s`.  The numbers here reflect the order: since not all languages follow the same word-order as English, you can change the placeholder order.
  1. Save your work in the `lang` directory as a `.po` file named after your language and local, e.g. `en_US` for American English or `pt_BR` for Brazilian Portuguese.  PoEdit will create both `.po` and a `.mo` versions of the file.
  1. If you haven't already, open a ticket in the Issues tab and attach your `.mo` and `.po` files and request that these be included in the next version.

# Customizing a Translation #

If you need to customize things further, you can change your site's configuration.  Copy the `config/lang/dictionaries.php` file to your `wp-content/uploads/cctm` directory (the CCTM will attempt to create this directory when it installs, but you may have to create it manually).

You can override the contents of this file and point it to your own new translation files.  This allows you to update any translations without overwriting the managed translations inside the plugin's directory.