One of the things that I felt was at fault in the way WordPress deals with custom fields is how it stores the data.  The database tables are related in a fairly standard way: **wp\_posts** contains the primary post ID, and the **wp\_postmeta** table contains the custom field names and values keyed back to that post ID.

WordPress stores an array of values in multiple rows. Given a single post ID and a single field name, there could be an infinite number of rows in the database: the field name in the **wp\_postmeta** is not unique.

This represents a problem when it comes to searching on and paginating result sets that stretch across these 2 database tables.   It is _extremely_ difficult to retrieve a set of posts and all their custom fields using a _single_ database query.  MySQL just doesn't bend that way -- you can't join tables on an indeterminate number of rows.  That means that it can be very difficult to search and filter result sets when you are searching and filtering based on custom fields.

So I took aim at WordPress' strategy here and through the development of my Summarize Posts plugin, I arrived at a different strategy: one row in **wp\_postmeta** per custom field.  Period.  Arrays of values (e.g. multiple values from a multi-select field) are stored as JSON-encoded strings.

Storing data structures as serialized strings or JSON-encoded strings is fairly common.  Note that this happens ONLY when you are storing data from a "repeatable" field (e.g. a multi-select storing multiple values).  Starting in version 0.9.5, this same approach is used to store repeatable image, text, or relation fields. It's used for all fields that store an array of data (relational data or not).

WordPress does something similar to store metadata for its uploaded images, but WordPress _serializes_ the data instead of JSON-encoding it.  The JSON format is actually queriable (whereas PHP serialized data generally is not).  E.g.

```
SELECT * FROM wp_postmeta WHERE post_id=123 AND meta_key='my_custom_field' AND meta_value LIKE '%"7"%';
```

Note the quotes are included in the LIKE query.  Since the **wp\_postmeta** table is not indexed, MySQL will be forced to do a table scan to find the rows (i.e. it's a slower query) -- so in other words, there's no disadvantage to doing a "LIKE" query because the table needs to get scanned either way.  I'm building more functionality into the [Summarize Posts](http://code.google.com/p/wordpress-summarize-posts/wiki/get_posts) plugin, so you should be able to handle most queries you can think of.

So the other thing that made me go for the JSON approach was that WordPress offers no way of sorting its meta values.  When you use the **get\_post\_meta()** function to retrieve an array of values from the **wp\_postmeta** table, you don't have any control over the order that they come back out of the array, whereas with the CCTM approach, you can simply shuffle the input fields as they are stored, save the form, and back in your templates, the order of the values updates (see this in action in version 0.9.5).

Was this the correct approach? I don't know.  I think I made the decision that was the lesser of 2 evils.  Maybe it would have been better to stick to WP's approach and just suck up the fact that performance would suffer and I'd need to make multiple database queries, but I've had to do that for some other projects (unrelated to WP), and the searching, pagination, etc. just got to be completely untenable if you couldn't grab all your data in a single query.   I'm not the only guy who's used this solution: MODX uses this for its custom fields, and I've spotted the same approach in parts of Drupal and Expression Engine, so **somebody** thinks it's viable.   Perfect?  No.  But a viable solution for a majority of use-cases.