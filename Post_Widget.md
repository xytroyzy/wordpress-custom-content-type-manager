# Introduction #

The Post Content Widget lets you include the content (or any field) from a single post and display the result in a widget.

![https://img.skitch.com/20120823-f74r68gxyc2e3tmeq8jhf1134b.jpg](https://img.skitch.com/20120823-f74r68gxyc2e3tmeq8jhf1134b.jpg)

<a href='http://www.youtube.com/watch?feature=player_embedded&v=uejIkssRfSM' target='_blank'><img src='http://img.youtube.com/vi/uejIkssRfSM/0.jpg' width='425' height=344 /></a>


# Settings #

  * **Post Type:** This dropdown will limit the choices available to you when you click the "Choose Post" button.
  * **Choose Post:** clicking this button will launch a lightbox that will allow you to browse the available posts, pages, etc. (depending on the value set in the "Post Type" dropdown).  Select the post you want.
  * **Override Post Title:** by checking this box, you can type your own value into the "Title" field; if unchecked, the title of the selected post will be used for the title of the widget.
  * **Title:** The value in this text field will appear above the widget if "Override Post Title" is checked.
  * **Formatting String:** this is what formats each result returned.  _It should be a list item_!

The default formatting string is simply this:

```
[+post_content+]
```

Note that the formatting string uses `[+placeholders+]` that get replaced with data, just as the formatting templates do when you [customize the Manager HTML](CustomizingManagerHTML.md).  Any field available to the posts you are listing will have a  corresponding placeholder.  This includes all built-in columns to the wp\_posts table AND any custom fields you have defined.  You can use the `[+help+]` placeholder to print out any available placeholders for you.