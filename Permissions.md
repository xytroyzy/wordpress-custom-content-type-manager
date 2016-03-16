# Introduction #

Most of the time, the CCTM plugin doesn't need to write anything to the filesystem on your webserver, however with the addition of support for [Importing](Import.md) and [Exporting](Export.md) content type definitions, it can save you a lot of time and trouble to be able to save your definitions on your webserver without having to upload or download them repeatedly.

The ability to write to the file system is _not_ essential for normal operation of the plugin. It merely makes for a better user experience.


# Errors #

If PHP warnings are being displayed, you might see a warning like this:
```
Warning: mkdir() [function.mkdir]: Permission denied in 
/path/to/wp/html/wp-content/plugins/custom-content-type-mgr/includes/CCTM.php
```

But it should be accompanied by a more verbose message indicating which directory it failed to create:


The solution is to change the permissions on the directory specified in the error message to something more relaxed.