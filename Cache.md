CACHING FUNCTIONALITY IS IN DEVELOPMENT -- it can cause as many problems as it solves.

## Directory Scans ##

## Images ##

Thumbnail images in the post-selector are cached: the built-in WordPress for this simply did not work, so if you had uploaded a bunch of huge images, then the post-selector would load _all_ of them in their _full_ sizes.  So wrote my own thumbnail generation script.... the thumbnails are pretty poor quality, but using tiny images does not strain the manager. You can always preview the full-sized image.

### Location ###

Images are cached inside a sub-folder of your uploads directory:

`wp-content/uploads/cctm/cache/images`