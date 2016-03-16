WordPress must register all custom post\_types using the [register\_post\_type()](http://codex.wordpress.org/Function_Reference/register_post_type) function in order for WordPress to recognize them.

_None of your content will ever be deleted from the database via deactivation_, but if a post\_type is not registered, it is essentially becomes invisible to the WordPress application.  All the posts and their custom fields remain in the database, but you will no longer be able to view or edit them.  Any URLs that point to pages using this custom content type will 404.

Reactivating the content type will cause WordPress to "see" all of your posts using that custom content type once more.

Compare this to [deleting a content type](DeletePostType.md).