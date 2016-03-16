The actual **.cctm.json** files are json encoded text files, but they can be converted back into PHP objects which are used to power the CCTM plugin.

# Data Structure #

The structure of the `.cctm.json` definition files is a subset of the primary [Data Structure](DataStructures.md)

```
Array(
   [post_type_defs] => Array( ... ),
   [custom_field_defs] => Array( ... ),
   [export_info] => 
        (
            [title] => My Example
            [author] => Everett
            [url] => http://wpcctm.com/
            [description] => Contains pages for Books, Movies, and Actors
            [template_url] => http://www.themeforrest.com/my-theme 
            /* plus a few attributes for tracking data */
        )
)
```

## Data Modifications ##

There are a couple modifications to the data that exists in the full [Data Structure](DataStructures.md) that need to be made before the definition is portable.

  * Paths to menu icons are converted to relative paths (this applies only to local image paths, not to images hosted by a 3rd party)
  * Default values for any [Relation](Relation.md) fields ([Image](Image.md), [Media](Media.md)) are nulled out: this is because they point to a specific post in the _local_ database, so those references will have no meaning on another site with

# See Also #

[DataStructures](DataStructures.md) -- complete data structure