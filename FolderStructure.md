The CCTM was designed to be customized.  There are a series of optional folders that you can add to your site that will be scanned for various components to augment or override built-in CCTM functionality.



| ![http://s2.postimage.org/by3ngezo/warning_icon.png](http://s2.postimage.org/by3ngezo/warning_icon.png) | <font color='red'>WARNING</font>: do not edit the files in the `plugins/custom-content-type-manager/` directory. Any changes you make there will be destroyed when you update the plugin!|
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

The default location of the folders are inside of `wp-content/uploads/cctm/`


# defs/ #

This is where your library of CCTM definition files lives (**defs** for definitions).  When you save your definition to your library or your import a definition file, it ends up here.

`wp-content/uploads/cctm/defs/`


---


# fields/ #

This folder is for any [Custom Field Classes](CustomCustomFields.md) that you write yourself.


---


# filters/ #

This is where you can add any [Custom Output Filters](CustomOutputFilters.md)


---


# tpls/ #

This folder and its sub-directories is where formatting strings are stored.

## tpls/fields/ ##

### tpls/fields/elements/ ###

### tpls/fields/options/ ###

### tpls/fields/wrappers/ ###


## tpls/post\_selector/ ##

### tpls/post\_selector/items/ ###

### tpls/post\_selector/search\_forms/ ###

### tpls/post\_selector/wrappers/ ###

## tpls/summarize\_posts ##

This contains 2 templates:

  * search.tpl
  * widget.tpl

## tpls/widgets/ ##

  * post\_item.tpl


---


# validators/ #

This folder is where you can store [Custom Validators](CustomValidators.md).  Use these if you need to ensure that input is a certain format prior to publishing your posts and pages.