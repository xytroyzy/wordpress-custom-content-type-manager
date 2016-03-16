Starting with CCTM version 0.9.7.1, you can display a post-creation form on the front end of your site, similar to Gravity Forms.  This is a beta version with limited functionality only.  See the section below on Limitations.

| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | <font color='red'>Displaying forms on the front-end of your site is dangerous!  Forms are often the vector of XSS or SQL Injection attacks.  Use this feature with extreme caution!</font> |
|:--------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|




---


# Usage #

```
 [cctm_post_form post_type="movie"]
```

Or to send out an email:

```
 [cctm_post_form post_type="movie" _email_to="my@email.com" _email_tpl="32"]
```

Or to display a simple thank you message:

```
[cctm_post_form post_type="application"]
Thank you submitting your application!
[/cctm_post_form]
```

Send the form to a 3rd party:

```
[cctm_post_form post_type="newsletter" _action="http://mailchimp.com/submit/"]
```

It is possible to include multiple instances of this shortcode on a single page, but each instance must use a unique `_name_prefix` so that the form handler can identify which fields belong to it.  Relying on this is not recommended, however -- it just gets too confusing to have multiple forms on the same page.


---


# Arguments #

## Required Arguments ##

  * post\_type : you need to specify which post-type you want to use. This tells the CCTM which custom-fields it needs to draw.


---


## Control Arguments ##

After submitting the form, generally you want a couple things to happen: you want a thank you page to be displayed or you want to be emailed that a form was submitted etc.  The shortcode accommodates these use cases.  Unlike the other arguments, control arguments are prefixed with an underscore (`_`).

### `_callback_pre` ###

(_string_) Name of a callback function to be called before the form is drawn.  The function can accept an array with keys for each of the arguments in the shortcode and for each field name in the submitted form.

Use this if you need to vet the visitors before they fill out the form: any output this function returns will be added to the `[+content+]` placeholder.  Default: null.

Your function should accept an array: this will be an array of the arguments passed to the shortcode and any calculated defaults (e.g. "post\_status").  You can use this function to prevent the form from being drawn by redirecting to another page, e.g.

```
// Redirect to page 123
$url = get_permalink(123);
CCTM::redirect($url, true);
```

Note that this uses a Javascript redirect due to limitations in the WordPress architecture (PHP redirects are impossible because WordPress has already sent headers).


### `_callback` ###

(_string_) Name of a callback function to be called _after_ all fields have been validated and any name prefixes have been stripped.  The function should accept an array of arguments (data is merged from posted data or hard-coded arguments in your shortcode).  This function is called **after successful validation**.  The callback function should accept a single array of key/value pairs.  It will receive all arguments passed to the  shortcode and all posted data, filtered via the [wp\_kses](http://codex.wordpress.org/Function_Reference/wp_kses) filter (see section below on Limitations).  Although the keys in the `$_POST` array include the prefix specified by the `_name_prefix` argument, this prefix is removed before the array is passed to the callback function.

Default is **CCTM::post\_form\_handler**.  That handles creating a new post out of the data submitted.

### `_tpl` ###

(_string_) references a template file in `uploads/cctm/tpls/forms/`.  There is a default template available.  You can also specify a template file that matches the name of the post\_type, prefixed with an underscore, e.g. `_my_post_type`.  If you use your own form tpl, be sure to use either the `[+content+]` placeholder (containing _all_ fields), or include the `[+nonce+]` placeholder.  Otherwise form submission will fail.

### `_action` ###

(_string_) used in the form template, where the form is submitted.  Default is the URL of the current page.  If you use a custom action parameter (e.g. to post a form to a 3rd party), then most of the other control behaviors are ignored: the form will not be validated, data will not be stored in the database, and notification emails will not go out.

### `_id_prefix` ###

(_string_) used in form elements to prefix the CSS classes.

### `_name_prefix` ###

(_string_) used to prefix the field names, which affects the keys of the `$_POST` array, e.g. a prefix of `cctm_` and a field name of "yourfield" come out like this:

```
<input type="text" name="cctm_yourfield" />
```

This prefix is stripped from keys in the `$_POST` array when the form is submitted: any callback function will not see the prefix.

### `_css` ###

(_string_) Comma-separated list of URLs for any additional CSS file(s) you wish to include on the form page.

Defaults are the CCTM's`css/validation.css` and `css/manager.css` CSS files.

### `_fields` ###

(_string_) Comma-separated list of fields to display.  Use this only when you need to display only some of the fields defined for your post-type.  Be careful: if your post-type has required fields and you do not display those fields, you will never be able to save the posted data!






---


## Labeling Arguments ##

Sometimes you want to display the built-in WordPress fields (the title, content, or excerpt) using a different label.  You can specify the following arguments to override the default WordPress labels:

  * `_label_title`
  * `_label_content`
  * `_label_excerpt`



---


## Extra Control Arguments ##

The following arguments are used by the default callback function.  If you are using your own callback function, you may very well wish to supply your own arguments (just prefix them with an underscore), and let your callback function handle them appropriately.

### `_msg` ###

(_string_) optionally message shown to users after successful submission of the form.  The string can also be specified by using the full-shortcode with opening and closing tags:

```
[cctm_post_form post_type="abc" _msg="Your message here"]
```

Is equivalent to:

```
[cctm_post_form post_type="abc"]
Your message here
[/cctm_post_form]
```

The message shows only if the `_redirect` option is not used.


### `_redirect` ###

(_integer_) optionally, you can set a page ID here where you want the user to be redirected to after any other functionality is completed (i.e. after updating the database and sending an optional email).  Typically this is a "Thank You" page.

## Email Arguments ##

You can use this form to send emails.  This could be used to alert the blog owner when a new person has entered their info, or email a sales rep when a question has been submitted.  There are lots of options.

### `_email_to` ###

(_string_) required comma-separated list of email addresses where notifications should be sent when a form is submitted (see [wp\_mail](http://codex.wordpress.org/Function_Reference/wp_mail) for valid formats).  If you set this, you must provide the `_email_tpl` argument to specify the content and format of your email.

### `_email_from` ###

(_string_) optional email address to be used as the "from" address.  The format may be one of the following:

  * `user@email.com` for a simple email address.
  * `'My Name' <myname@mydomain.com>` for a distinct name and email.
  * `&#034;My Name&#034; <myname@mydomain.com>` or use this format for a distinct name and email.

Do not include the "From: " -- that is included automatically).

If omitted, this defaults to the admin\_email setting (Settings --> General).

### `_email_subject` ###

(_string_) optional subject for your email.  If omitted, this will default to the `post_title` of the page referenced by the `_email_tpl` argument.

### `_email_tpl` ###

(_integer_) required page ID of the page containing the text of your email.  The page will be parsed for any `[+placeholders+]` corresponding to values submitted on the form.  For example, if your form includes a custom field for `first_name`, then your email template (tpl) might include something like `[+first_name+] submitted the form!`.

The built-in callback does not support dynamic email addresses: it sends only to the email address(es) supplied in the shortcode: this is to cut down on the risk that the form be used to send spam emails.


### `_email_only` ###

(_integer_) 1|0.  If set to 1, a new post will not be created, and only an email will go out (this requires that both the `_email_to` and `_email_tpl` be set.  Using this option is not usually recommended: email is inherently unreliable, and there is no way to verify its receipt.  It's usually a good idea to store data in the database as well.


---


## Default Arguments ##

It's extremely important that you watch your content here.  By default, the post\_status is set to be "draft" in order to force all posts created via this method to be in draft status so that they will REQUIRE admin approval before being publicly visible.

  * post\_status : "draft"
  * post\_author : current user id (if available)
  * post\_date : result of PHP's `date('Y-m-d H:i:s')` function
  * post\_date\_gmt : result of PHP's `gmdate('Y-m-d H:i:s')` function
  * post\_modified : result of PHP's `date('Y-m-d H:i:s')` function
  * post\_modified\_gmt : result of PHP's `gmdate('Y-m-d H:i:s')` function


| ![http://s15.postimage.org/s5mvsou53/info.png](http://s15.postimage.org/s5mvsou53/info.png) | If you specify a value via the shortcode argument, **no form element will be drawn**.  There's no reason to prompt the user for a value if you're hard-coding it. |
|:--------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Optional Arguments ##

Any other field value can be specified manually in your shortcode arguments, and these values will override anything posted in the form.  For example, if you want each form submitted on the front-end to be specially flagged, you might have a custom field named "source" which you can set via your shortcode call:

```
 [cctm_post_form post_type="feedback" source="front-end"]
```

Any built-in WordPress column is available as a shortcode attribute, as well as any custom field name you've defined for your post-type (see the limitations below).


---


# Custom Formatting #

Just like in the manager, the field generation relies on formatting templates (tpls).  You can specify a custom HTML template by using the `_tpl` argument.  The argument should correlate to a file inside `uploads/cctm/tpls/forms/`, e.g. `_tpl="myform"` would load up `uploads/cctm/tpls/forms/myform.tpl` to format the form output.

At a minimum, your formatting template (tpl) should contain the following:

```
<form action="[+_action+]" method="post">
	[+content+]
</form>
```

Where `[+content+]` will contain all fields generated by the form-generation process.  You may also include each field individually by a placeholder matching their name, e.g. `[+myfield+]` or `[+first_name+]`.  If you list form fields verbosely, make sure you include a placeholder for `[+nonce+]`: this will include the nonce security field.  <font color='red'>Your form submission will fail if you omit the <code>[+nonce+]</code> placeholder!</font>

See [Customizing the Manger HTML](CustomizingManagerHTML.md) page for information.


---


# Limitations #

Letting the general public submit content to your site is potentially **very** dangerous.  There are some limitations here that exist specifically for the protection of you and your site.  In future versions some of these restrictions may be relaxed as safe work-arounds are developed.

## Shortcode Attributes ##

There is a bug (a "feature"?) in WordPress: <font color='red'><b>all</b> shortcode parameters must use only lowercase letters and underscores.</font>  If you try to pass custom field names as shortcode arguments and the attributes use upper-case letters or numbers, the shortcode will not work!  This is not well-documented!

This restriction is for your field NAME, not for the LABEL.  The field name is what identifies it uniquely in the database, not what you display to your users in the WordPress admin.  And this limitation only comes up if you need to hard-code a value for your field as a shortcode argument.

For example, I have a post\_type named "feedback" with a custom field named "Big5".  The following shortcode would fail:

```
[cctm_post_form post_type="feedback" Big5="I am lost"]
```

WordPress will misinterpret and rename the "Big5" variable, so when CCTM tries to find a value for your "Big5" field, it will not find the value.

The solution is to use a different name for your field, e.g. "big\_five".  Then you can use the shortcode normally:

```
[cctm_post_form post_type="feedback" big_five="I am found"]
```

## Post Content and WYSIWYG ##

Any WYSIWYG field, including the main content editor (ie. post\_content field) relies on the TinyMCE editor with numerous plugins that let you do things such as upload media or reference other posts.  For security reasons, this cannot be supported on the front-end!  You do not want the general public to have this kind of access to other parts of your site!

Unfortunately, the WYSIWYG editor that WordPress uses inside the manager is not easily implemented in other areas of the site.

For now, only plain textarea fields can be used for WYSIWYG fields.  In the future, the CCTM may integrate a simplified WYSIWYG editor for the post\_content and any WYSIWYG fields, e.g. https://code.google.com/p/jwysiwyg/wiki/Introduction

## Repeatable Fields ##

Because repeatable fields require special Ajax requests to handle the creation and deletion of form elements dynamically, they are not currently supported.  Please use simple fields for this feature.

## Image, Media, and Relation fields ##

For security reasons, the form does not currently support these fields: image and media fields are not supported because they would offer the general public the unfettered ability to upload assets to your server, and relation fields are not supported because they may list posts that are not intended for public viewing (e.g. posts in draft status or posts in a non-public post\_type).

## Filtering ##

When content is submitted via the **cctm\_post\_form** shortcode from the front-end of a site, the [wp\_kses](http://codex.wordpress.org/Function_Reference/wp_kses) filter is applied to **all** inputs.  This is a stricter filter than what is used inside the WordPress manager: it filters out all HTML.  (Inside the manager, the post\_content normally uses the more relaxed [wp\_kses\_post](http://codex.wordpress.org/Function_Reference/wp_kses_post) filter).

Since this form is shown on the front-end, the source of the data is considered untrusted.