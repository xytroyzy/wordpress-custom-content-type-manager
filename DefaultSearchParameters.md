Starting with version 0.9.5, you can configure the default search parameters that define which posts are available for selection in a relation, image, or media field.  Note that you can also configure the available [Search Parameters](SearchParameters.md) that your managers can define when creating a custom relation, media, or image field.  The functionality overlaps here quite a bit

# Configuring the Default Search Parameters #

You can override the default configurations by writing your own configuration file.

Inside of your WordPress uploads directory, the CCTM uses its own directory, e.g. `wp-uploads/cctm` -- that's where we put all of our CCTM stuff.

The little secret is that any configuration file inside of **custom-content-type-manager/config/post\_selector** can be overridden by placing a file into **uploads/cctm/config/post\_selector**.  That is to say, the user-created files inside of your upload directory take precedence over the the included built-in configurations.


Sample contents:

```
<?php

CCTM::$post_selector['search_columns'] = array('post_title', 'post_content');
CCTM::$post_selector['post_type'] = 'attachment';
CCTM::$post_selector['post_mime_type'] = 'image';
CCTM::$post_selector['post_status'] = array('publish','inherit');
CCTM::$post_selector['orderby'] = 'ID';
CCTM::$post_selector['order'] = 'DESC';
CCTM::$post_selector['limit'] = 10;
CCTM::$post_selector['paginate'] = 1;

?>
```