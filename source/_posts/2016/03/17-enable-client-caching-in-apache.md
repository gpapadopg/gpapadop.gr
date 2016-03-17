---
title: Enable client caching in Apache
tags:
    - Apache
---

> Fetching something over the network is both slow and expensive: large responses require many roundtrips between the client and server, which delays when they are available and can be processed by the browser, and also incurs data costs for the visitor. As a result, the ability to cache and reuse previously fetched resources is a critical aspect of optimizing for performance.

via [Google Developers](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching).

For apache webserver, create a new file `mods-available/expires.conf` with your favorite editor:

	vi mods-available/expires.conf

and put the following contents in it:

	<IfModule mod_expires.c>
	 	ExpiresActive On
		ExpiresByType image/jpg "access plus 1 month"
		ExpiresByType image/png "access plus 1 month "
		ExpiresByType image/gif "access plus 1 month"
		ExpiresByType image/jpeg "access plus 1 month"
		ExpiresByType text/css "access plus 1 month"
		ExpiresByType image/x-icon "access plus 1 month"
		ExpiresByType application/pdf "access plus 1 month"
		ExpiresByType audio/x-wav "access plus 1 month"
		ExpiresByType audio/mpeg "access plus 1 month"
		ExpiresByType video/mpeg "access plus 1 month"
		ExpiresByType video/mp4 "access plus 1 month"
		ExpiresByType video/quicktime "access plus 1 month"
		ExpiresByType video/x-ms-wmv "access plus 1 month"
		ExpiresByType application/x-shockwave-flash "access 1 month"
		ExpiresByType text/javascript "access plus 1 month"
		ExpiresByType application/x-javascript "access plus 1 month"
		ExpiresByType application/javascript "access plus 1 month"
	</IfModule>

Afterwards, enable it with:

	a2enmod expires

and restart apache:

	service apache2 restart

Of course you may alter the values presented above. Optionally, you can disable the `ETag` header with:

	echo "
	Header unset ETag
	FileETag None
	" > /etc/apache2/conf.d/unset-etag.conf
