---
title: Optimizing images for use on the web
tags:
    - Web

---

The importance of image optimization from [google developer](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization#optimizing-raster-images):

> Images often account for most of the downloaded bytes on a web page and also often occupy a significant amount of visual space. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for the client's bandwidth and the faster the browser can download and render useful content on the screen.

After this short intro, I mainly use:

[jpegoptim](https://github.com/tjko/jpegoptim): utility to optimize jpeg files. Provides lossless optimization (based on optimizing the Huffman tables) and "lossy" optimization based on setting maximum quality factor.

	# example command line usage to optimize images under a directory
	find /path/to/dir -type f -name "*.jpg" -exec jpegoptim --strip-all {} \;

[optipng](http://optipng.sourceforge.net/): PNG optimizer that recompresses image files to a smaller size, without losing any information. This program also converts external formats (BMP, GIF, PNM and TIFF) to optimized PNG, and performs PNG integrity checks and corrections. 

	# example command line usage to optimize images under a directory
	find /path/to/dir -type f -name "*.png" | xargs optipng -nc -nb -o7 -full

If you are using [grunt](http://gruntjs.com/) on your development workflow, look for the
[grunt-contrib-imagemin](https://github.com/gruntjs/grunt-contrib-imagemin) task
and apply the options like:

    imagemin: {
        png: {
            options: { optimizationLevel: 7 },
            files: [...]
        },
        jpg: {
            options: { progressive: true },
            files: [...]
        }
    }