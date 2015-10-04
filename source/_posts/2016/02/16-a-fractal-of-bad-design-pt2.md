---
title: A fractal of BAD developers Pt.2
tags:
    - PHP

---

After populating the database with data and putting the files
in place, we found out that the developers have developed the site with 
register_globals on, so instead of something like:

	$id = $_GET["id"];

every variable was defined in place and used immediatelly:

	$result1=mysql_query("select * from TABLE where id='$id'");

Yes, really! This `$id` may be from SESSION, from a COOKIE, from POST, from GET, 
from Mars, you name it!

And because support for [register_globals](http://php.net/manual/en/security.globals.php)
have long gone, the consequense was that we couldn't see any other page properly. 
At least on pages with query parameters in URL. And with hundreds of files
it was a hell lot of a job to correct it.

After digging around, we finally apply this snippet of code from [http://php.net/manual/en/faq.misc.php#faq.misc.registerglobals](http://php.net/manual/en/faq.misc.php#faq.misc.registerglobals):

	<?php
	// Emulate register_globals on
	if (!ini_get('register_globals')) {
	    $superglobals = array($_SERVER, $_ENV,
	        $_FILES, $_COOKIE, $_POST, $_GET);
	    if (isset($_SESSION)) {
	        array_unshift($superglobals, $_SESSION);
	    }
	    foreach ($superglobals as $superglobal) {
	        extract($superglobal, EXTR_SKIP);
	    }
	}

by using the `auto_prepend_file` directive like this:

	php_admin_value[auto_prepend_file] = "/path/to/register-globals.php"

To be continued...