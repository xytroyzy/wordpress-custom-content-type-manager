

# Introduction #

The CCTM\_Pagination class is a library for generating pagination links (revised for version 0.9.6.2 of the CCTM).  It is used by the GetPostsQuery class, for example when you select a post or image in a custom field:

![http://content.screencast.com/users/fireproofsocks/folders/Jing/media/f0d80440-d3d4-4f3d-a776-acbd94a48e9f/00000107.png](http://content.screencast.com/users/fireproofsocks/folders/Jing/media/f0d80440-d3d4-4f3d-a776-acbd94a48e9f/00000107.png)

The links generated are typical CGI style, with a URL parameter for `&offset`, something like this:

```
<a href="?&offset=25" >2</a>
<a href="?&offset=50" >3</a>
<a href="?&offset=75" >4</a>
<!-- ... etc... -->
```

It is possible to configure your web application to respond offsets supplied via the URL instead of inside the query string, but that's an exercise in server configuration and in most cases, it is needlessly complex.


---


# Methods #

### <font color='green'>public function offset_to_page()</font> ###

_integer_ **offset\_to\_page**( integer _$offset_ `[`, integer _$results\_per\_page_`]`)

  * **$offset** _integer_
  * **$results\_per\_page** _integer_ -- if omitted, the class' results\_per\_page value will be used.
  * **OUTPUT**: _integer_

This is a convenience function for converting between an offset number and a page number.  The internal functions and database queries rely on offsets, but humans prefer page numbers.

### <font color='green'>public function page_to_offset()</font> ###

_integer_ **page\_to\_offset**( integer _$offset_ `[`, integer _$results\_per\_page_`]`)

  * **$page** _integer_
  * **$results\_per\_page** _integer_ -- if omitted, the class' results\_per\_page value will be used.
  * **OUTPUT**: _integer_

This is a convenience function for converting between a page number and an offset number.  The internal functions and database queries rely on offsets, but humans prefer page numbers.


### <font color='green'>public function paginate()</font> ###

_string_ **paginate**( integer _$record\_count_)

  * **$record\_count** : the number of records you are paginating.
  * **OUTPUT**: HTML representing pagination links.

This is THE primary function of this entire class.  See the examples below.


### <font color='green'>public function set_base_url()</font> ###

_void_ **set\_base\_url**( string _$url_ )

  * **$url** _string_  any valid URL, relative or absolute.

Set the URL to be used as the base for the links.  This can be relative or absolute.  If you are not using friendly URLs and you have some CGI parameters that you wish to persist between pages, then you must include these when you set the base url.

For relative URLs, you may wish to simply use "?".

If your pagination occurs on a page that is reliant on other CGI query variables (e.g. if you have permalinks disabled), then you must include these when you set the base url.  Any URL parameter that you wish to remain constant across pages needs to be passed to this function.

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | Sanitize the URL before setting the base url! The `set_base_url()` function does not filter values!  Do not pass unsanitized data to this function or you will be vulnerable to XSS attacks! |
|:--------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|


### <font color='green'>public function set_jump_size()</font> ###

_void_ **set\_jump\_size**( integer _$pgs_ )

  * **$pgs** _integer_  greater than zero which defines the number of pages that are advanced when the "Next" or "Previous" links are clicked.

Most commonly, the pagination flow is like that of a book: clicking on the "Next" or "Previous" links will flip forwards or backwards one page at a time.  However, in some scenarios (e.g. with very large record sets), you may with to "fast-forward" to an entirely new set of links each time the "Next" or "Previous" links are clicked.  In those cases, set the jump size to a larger number, e.g. to a number equal to the link\_cnt (see `set_link_cnt()`).


### <font color='green'>public function set_link_cnt()</font> ###

_void_ **set\_link\_cnt**( integer _$cnt_ )

  * **$cnt** _integer_  greater than zero which determines the number of pagination links displayed.

This class uses a default value of 10 links. Here is an example of using a link\_cnt of 5:

![http://content.screencast.com/users/fireproofsocks/folders/Jing/media/304b3f14-a09b-4ea9-9114-e2c51f20cc64/00000116.png](http://content.screencast.com/users/fireproofsocks/folders/Jing/media/304b3f14-a09b-4ea9-9114-e2c51f20cc64/00000116.png)

### <font color='green'>public function set_offset()</font> ###

_void_ **set\_offset**( integer _$offset_ )

  * **$offset** integer defining the offset used in the result set.

This value should be dynamically read out of the URL so that the page containing the pagination can dynamically react to the values being passed to it.  The default value here is zero which is equivalent to being on the first page of results.

```
// Dynamically read the offset
if (isset($_GET['offset'])) {
	$offset = (int) $_GET['offset'];
}
$P = new CCTM_Pagination();
$P->set_offset($offset);
$P->paginate(1000); 
```


### <font color='green'>public function set_results_per_page()</font> ###

_void_ **set\_results\_per\_page**( integer _$results\_per\_page_ )

  * **$results\_per\_page** integer greater than zero which defines how many results are displayed on each page of results.

Knowing the number of results displayed on each page is critical in calculating the pagination links.  100 results, for example, would require 10 pages if $results\_per\_page=10, but only 4 pages if $results\_per\_page were set to 25.

It is important to understand that the CCTM\_Pagination does not display results directly, so setting this value is more of a "courtesy call".  In order to ensure accurate pagination links, however, you should use the $results\_per\_page value that is equal to the number of results you are actually printing on each page.

### <font color='green'>public function set_tpl()</font> ###

_void_ **set\_tpl**( string _$tpl_, string _$content_ )

  * **$tpl** string name of one of the component formatting templates (tpls): firstTpl,lastTpl,prevTpl,nextTpl,currentPageTpl,
> > pageTpl,outerTpl
  * **$content** string containing placeholders.  See the Tpls section below.

Use this function to set the content of a tpl.


### <font color='green'>public function set_tpls()</font> ###

_void_ **set\_tpls**( array _$tpls_ )

  * **$tpls** associative array containing key/value pairs where each key corresponds to one of the named formatting templates (tpls): firstTpl,lastTpl,prevTpl,nextTpl,currentPageTpl,
> > pageTpl,outerTpl

Use this function to set all tpls in one swoop.  If you omit any of the keys, it is equivalent to setting the corresponding tpl to an empty string.



---


# Formatting Templates (tpls) #

Pagination links are generated by assembling together a series of formatting strings (a.k.a templates or "tpls" for short).

This text graphic shows some sample links and the formatting tpl that applies to each part of the output:

```
	<<First <<Prev 1 2 3 Next>> Last>>
	\_____/ \____/ ^ ^ ^ \____/ \____/
	   |       |   | | |    |      +----- lastTpl
	   |       |   | | |    +------------ nextTpl
	   |       |   | | +----------------- currentPageTpl
	   |       |   +-+------------------- pageTpl
	   |       +------------------------- prevTpl
	   +--------------------------------- firstTpl

\_________________________________________________/
                    |
                    +-------------------- outerTpl
```

You can set the tpls individually using the `set_tpl()` function or all at once using the `set_tpls()` function.

### Placeholders ###

The following placeholders are available throughout the tpls.  There are some others available only to the **outerTpl** (see below).

  * `[+base_url+]` : This is the URL of the page containing your get\_posts query.
  * `[+offset+]` : Contains an integer representing the offset.  You must read this from the URL and pass it back into your get\_posts arguments.
  * `[+page_number+]` : instead of an offset number, this will print an integer for a page, e.g. when used in the **currentPageTpl**, it will contain the number of the current page, when used in the **nextTpl** it will contain the number for the next page, etc.

It should be pointed out here that _any_ value set on the CCTM\_Pagination object will be available in a corresponding placeholder.  For example:

```
$P->extra = 'onclick="alert('hiya');";
```

would populate any instances of `[+extra+]` with the bit of javascript.

## firstTpl ##

Used to format the link that takes you back to the very first page of results.

**Sample:**

```
<a href="[+base_url+]&offset=[+offset+]">&laquo; First</a> &nbsp;
```

## lastTpl ##

Used to format the link that takes you back to the very last page of results.

**Sample:**

```
&nbsp;<a href="[+base_url+]&offset=[+offset+]">Last &raquo;</a>
```


## prevTpl ##

Used to format the link that takes you back one page.

**Sample:**

```
<a href="[+base_url+]&offset=[+offset+]">&lsaquo; Prev.</a>&nbsp;
```

## nextTpl ##

Used to format the link that takes you forward one page.

**Sample:**
```
&nbsp;<a href="[+base_url+]&offset=[+offset+]">Next &rsaquo;</a>
```

## currentPageTpl ##

Used to format the number representing the current page.  You don't need to link to the page you're already on.

**Sample:**
```
&nbsp;<span>[+page_number+]</span>&nbsp;
```

## pageTpl ##

Used to format the links that take you directly to a given page.

**Sample:**
```
&nbsp;<a href="[+base_url+]&offset=[+offset+]">[+page_number+]</a>&nbsp;
```

## outerTpl ##

Wraps the final output.  This has a  couple special placeholders.

  * `[+content+]` : you **must** use this placeholder.  All other links and HTML will replace this placeholder.
  * `[+current_page+]` : contains the number of the current page. Useful for messaging the user.
  * `[+page_count+]` : shows the total number of pages in the result set.
  * `[+first_record+]` : number representing the first record visible on this page.
  * `[+last_record+]` : number representing the last record visible on this page.
  * `[+record_count+]` : number representing the total number of records in the result set.

**Sample:**

```
<div id="pagination">
	[+content+]<br/>
	Page [+current_page+] of [+page_count+]<br/>
	Displaying records [+first_record+] thru [+last_record+] of [+record_count+]
</div>
```


---


# Arguments #

It's important to review the arguments to the CCTM's [get\_posts](get_posts.md) function: some of those arguments directly affect pagination.  You don't have to use GetPostsQuery to use the CCTM\_Pagination class, it's just common, so we demonstrate it here.

## get\_posts ##

  * limit (int) : this limits the number of records returned by a single query AND it sets the number or records visible on each page.
  * offset (int) : this controls the offset in the !MySQL query. For creating a page containing dynamic paginated results, you should read the offset out of the URL parameters and pass it back into your
  * paginate (boolean) : set this to true if you want to paginate your results.




---


# Examples #

## 1. Simplest Example ##

The simplest invocation of this class isn't practically useful, but it should bring home the point that all this class needs is a number representing a count of the records to be paginated.

```
// In a template file
require_once(CCTM_PATH.'/includes/CCTM_Pagination.php');
$P = new CCTM_Pagination();
print $P->paginate(1000);
```

The result will be something like this:

![http://content.screencast.com/users/fireproofsocks/folders/Jing/media/5444f4f3-a30e-49df-9711-3f9aeff06b93/00000115.png](http://content.screencast.com/users/fireproofsocks/folders/Jing/media/5444f4f3-a30e-49df-9711-3f9aeff06b93/00000115.png)

Note that this example relies on all the default tpls and it does not include any code to dynamically read the current page, so it is not terribly useful.

## 2. Simple Example ##

Here is a slightly more useful example where we can at least click through our pages and see our links update.

```
require_once(CCTM_PATH.'/includes/CCTM_Pagination.php');
$P = new CCTM_Pagination();

// Read the offset variable (VERY IMPORTANT!)
if (isset($_GET['offset'])) {
	$P->set_offset($_GET['offset']);
}

print $P->paginate(1000);
```

However, in most cases we still need to calculate the number of records dynamically instead of passing the `paginate()` function a hard-coded value.

## 3. Simplest Working Example ##

In order for the library to be used in real-world scenario we must dynamically read the offset (e.g. from the URL as shown in example 2) and we must have a valid calculation of the number of results.   Normally, we count the number of results by issuing a database query that counts the results, but you could also paginate results gleaned from an API or from a number of files etc -- the source of your data will determine how you will determine an accurate count of the results.

This example assumes that you are using the CCTM\_Paganation class inside a WordPress template file, so it is using the **$wpdb** global object to drive database queries, but the principle can apply no matter how you count results and retrieve data.

```
global $wpdb;

require_once(CCTM_PATH.'/includes/CCTM_Pagination.php');

$P = new CCTM_Pagination();

// Read the offset variable (VERY IMPORTANT!)
$offset = 0;
$results_per_page = 10;
$P->set_results_per_page($results_per_page);  // also pass this to your MySQL query's LIMIT

if (isset($_GET['offset'])) {
	$offset = (int) $_GET['offset'];
	$P->set_offset($offset);
}
// Count results manually
$cnt_query = "SELECT COUNT(*) as cnt FROM {$wpdb->posts}";
$results = $wpdb->get_results($cnt_query, ARRAY_A );
$result_cnt = $results[0]['cnt']; 

// Get the data 
$query = "SELECT * FROM {$wpdb->posts} LIMIT $results_per_page OFFSET $offset";
$results = $wpdb->get_results($query, ARRAY_A );
foreach ($results as $r) {
	print $r['post_title'] . '<br />';  // print whatever data you want
}

// Print the pagination links
print $P->paginate($result_cnt);
```

## Using with GetPostsQuery ##

The SummarizePosts and GetPostQuery classes rely on the CCTM\_Pagination class, so you do not need to directly instantiate it directly.  Some of the arguments passed to the `SummarizePosts::get_posts()` function or to the [GetPostsQuery::get\_posts()](get_posts.md) function will affect pagination settings.

If you want to create something more involved than the simple "next/prev" type of links, then you need to use more of the formatting strings.

This example creates a simple link for "Older posts" -- it uses only a small portion of the available formatting templates.

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | When reading variables out of the URL, make sure you sanitize your data!  Using `(int)` to type-cast your variable to an integer is strong protection against SQL-Injection attacks, e.g. look closely at this line in the example below: `$args['offset'] = (int) $_GET['offset'];` |
|:--------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|


```
require_once(CCTM_PATH.'/includes/GetPostsQuery.php');
$Q = new GetPostsQuery();

// Define the query
$args = array();
$args['post_type'] = array('post');
$args['limit'] = 20;
$args['paginate'] = true;

// Read the offset variable (VERY IMPORTANT!)
if (isset($_GET['offset'])) {
	$args['offset'] = (int) $_GET['offset'];
}

$results = $Q->get_posts($args);

foreach ($results as $r) {
    // print out a bit about each post in the result set... 
}

print $Q->get_pagination_links();

```


## Customizing Tpls ##

The class offers two functions to allow you control over the formatting templates (tpls) used to format the output:

  * **set\_tpl($tpl\_name, $content)**: set a single template (`$tpl_name`) to `$content`.  Valid `$tpl_name` arguments are any of the valid template names listed above: "firstTpl", "lastTpl", "prevTpl", "nextTpl", "currentPageTpl", "pageTpl", "outerTpl"
  * **set\_tpls($tpls)**: pass this function a _complete_ associative array of all templates to be used. If the array you pass omits any of the valid keys, it is equivalent to you setting a null value for that key.

```
require_once(CCTM_PATH.'/includes/CCTM_Pagination.php');
$P = new CCTM_Pagination();

// Set pagination tpls
$tpls = array (
	'firstTpl'		=> '<span class="linklike" onclick="javascript:change_page(1);">&laquo; First</span> &nbsp;',
	'lastTpl' 		=> '&nbsp;<span class="linklike" onclick="javascript:change_page([+page_number+]);" >Last &raquo;</span>',
	'prevTpl' 		=> '<span class="linklike" onclick="javascript:change_page([+page_number+]);">&lsaquo; Prev.</span>&nbsp;',
	'nextTpl' 		=> '&nbsp;<span class="linklike" onclick="javascript:change_page([+page_number+]);">Next &rsaquo;</span>',
	'currentPageTpl'=> '&nbsp;<span class="post_selector_pagination_active_page">[+page_number+]</span>&nbsp;',
	'pageTpl' 		=> '&nbsp;<span class="linklike" title="[+page_number+]" onclick="javascript:change_page([+page_number+]);">[+page_number+]</span>&nbsp;',
	'outerTpl' 		=> '<div id="pagination">[+content+]<br/>
		Page [+current_page+] of [+page_count+]<br/>
	</div>',
);
$P->set_tpls($tpls);

print $P->paginate(1000);
```

## Customizing Tpls in GetPostsQuery ##


You can set the formatting of the formatting templates (tpls) when you use the GetPostsQuery class.

```
<?php
// in your template file...
$Q = new GetPostsQuery(); 

// ..add search filters to taste... 

// Set your formatting tpls
$tpls['firstTpl'] = '<a href="[+base_url+]&offset=[+offset+]" [+extra+]>&laquo; First</a> &nbsp;';
$tpls['lastTpl'] = '&nbsp;<a href="[+base_url+]&offset=[+offset+]" [+extra+]>Last &raquo;</a>';
$tpls['prevTpl'] = '<a href="[+base_url+]&offset=[+offset+]" [+extra+]>&lsaquo; Prev.</a>&nbsp;';
$tpls['nextTpl'] = '&nbsp;<a href="[+base_url+]&offset=[+offset+]" [+extra+]>Next &rsaquo;</a>';
$tpls['currentPageTpl'] = '&nbsp;<span>[+page_number+]</span>&nbsp;';
$tpls['pageTpl'] = '&nbsp;<a href="[+base_url+]&offset=[+offset+]" [+extra+]>[+page_number+]</a>&nbsp;';
$tpls['outerTpl'] = '<div id="pagination">[+content+]<br/>
				Page [+current_page+] of [+page_count+]<br/>
				Displaying records [+first_record+] thru [+last_record+] of [+record_count+]
			</div>';

$Q->set_tpls($tpls);

print $Q->get_pagination_links()
```