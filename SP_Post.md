The **SP\_Post** class offers functions to create, update, and delete a single post and its meta data (i.e. CRUD).  It does this by handling associative arrays passed to its functions (not by any persistent object state).

Managing your post and all related data via a single interface is simpler than the alternative of using iterations of the WordPress [wp\_insert\_post()](http://codex.wordpress.org/Function_Reference/wp_insert_post) and [update\_post\_meta()](http://codex.wordpress.org/Function_Reference/update_post_meta) and related functions.

Using **SP\_Posts** has these important distinctions:

  1. It does not call any actions or filters when it is executed, so for better or for worse, there is no way for 3rd parties to intervene with its behavior.
  1. It automatically creates/updates/deletes all custom fields in the postmeta table without the need to have to use the update\_post\_meta() and related functions.
  1. It does not check for user permissions. If you're mucking around in the PHP code and using this class, you have full run of the database. BEWARE!


---

# Methods #

---

## get ##

_array_ **get**( array _$args_)

**`$args` integer post id or an array of search arguments passed to [GetPostsQuery](GetPostsQuery.md)'s**get\_posts()**function.**

For example, you can use this to retrieve a single post by its ID:

```
$P = new SP_Post();
$post = $P->get(123);
```

Or you can use this to retrieve a single post that matches some search criteria:

```
$P = new SP_Post();
$args = array();
$args['post_type'] = 'animal';
$post = $P->get($args);
```

Remember: this always returns a single post.


---


## debug ##

_string_ **debug( )**

**_no arguments_**

This is a convenience function for aiding in debugging: call it to return a list of errors (formatted with HTML).

```
$SP = new SP_Post();
// do something.. maybe generate errors...

print $SP->debug();
```


---


## delete ##

_void_ **delete**( integer _$post\_id_ )

**`$post_id` : the id of the post you want to delete**

This deletes a post, its revisions, and its custom fields from the database.

```
// Delete post 123 from the database
$SP = new SP_Post();
$SP->delete(123);
```


---


## insert ##

_integer_ **insert**( array _$args_ )

  * `$args` an associative array representing all values for the post you are creating.

Returns false on error.

This inserts a post into the database.  Data is written into both the `wp_posts` table and the `wp_postmeta` tables.  Any array keys which do not correspond to columns in the `wp_posts` table will be considered custom fields (see the list of built-in columns at the bottom of the page).

The post ID of the newly created post is returned on success.

### Example 1 ###

```
$SP = new SP_Post();
$data = array();
$data['post_title'] = 'My Great Post';
$data['post_content'] = '<p>A long time ago in a galaxy far, far away...</p>';
$data['post_date'] = date('Y-m-d H:i:s');
$data['post_status'] = 'publish';
$data['post_type'] = 'post';  // or page, or custom_post_type
// Custom Fields
$data['rating'] = 'PG';
$data['language'] = 'English';

$post_id =  $SP->insert($data);

print "Created post $post_id";
```


---


## save ##

This switches between the insert/update functions based on whether or not the 'ID' key is present in the input arguments.  See the related functions for info.


---


## update ##

_integer_ **update**( array _$args_ `[`, boolean _$overwrite_ `]` )

  * `$args` are identical to arguments for the `insert` function, but they **must** include a key for `ID` (the post ID).
  * `$overwrite` (optional) If true, update will delete any custom fields not included in `$args`.

This method updates an existing post.  Set the `$overwrite` parameter to true if you want to ensure that no residual custom fields persist across updates.

### Example 1 ###

```
$Q = new GetPostsQuery();
$SP = new SP_Post();

// retrieve post 123
$my_post = $Q->get_post(123); 

$my_post['post_content'] = 'I changed the content!';
$my_post['my_custom_field'] = 'I changed/added a custom field!';

// Since we retrieved the post, $my_post['ID'] is already set
$SP->update($my_post);
```

### Example 2 ###

```
$SP = new SP_Post();
$data = array();
$data['ID'] = 456;
$data['post_title'] = 'Updated Title';
$SP->update($data);
```



---


# Built in Columns #

Columns from the `wp_posts` table.

  * ID
  * post\_author
  * post\_date
  * post\_date\_gmt
  * post\_content
  * post\_title
  * post\_excerpt
  * post\_status
  * comment\_status
  * ping\_status
  * post\_password
  * post\_name
  * to\_ping
  * pinged
  * post\_modified
  * post\_modified\_gmt
  * post\_content\_filtered
  * post\_parent
  * guid
  * menu\_order
  * post\_type
  * post\_mime\_type
  * comment\_count