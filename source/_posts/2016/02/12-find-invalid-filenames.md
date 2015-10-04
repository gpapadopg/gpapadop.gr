---
title: Find invalid filenames from a web host
tags:
    - Web

---

With the following code, you can find invalid filenames under
a directory:

	<?php
	// change according to your needs
	$dir = __DIR__;

	// force browser to show the output as plain text
	header('Content-Type: text/txt; charset=utf-8');

	// scan directory
	$dir = new DirectoryIterator($dir);
	foreach ($dir as $fileinfo) {
		// do not process . and ..
	    if ($fileinfo->isDot()) {
	        continue;
	    }
	    
	    $filename = $fileinfo->getFilename();

	    if (strlen($filename) == strlen(utf8_decode($filename))) {
	        continue;
	    }
	    
	    // filename mismatch
	    echo $dir . $filename . "\n";
	}

Obviously, to be used on a php web host.