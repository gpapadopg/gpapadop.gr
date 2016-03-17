---
title: Change wordpress domain
tags:
    - Wordpress
---

With the following snippet, you can change the domain/url of a wordpress site (no trailing slash):

	UPDATE wp_options SET option_value = 'http://my-domain' WHERE option_name IN('siteurl', 'home');

After that, you can use an extension like `wordpress move` in order to change the url on 
posts, pages, etc.

Useful when you are developing on a demo/test url.