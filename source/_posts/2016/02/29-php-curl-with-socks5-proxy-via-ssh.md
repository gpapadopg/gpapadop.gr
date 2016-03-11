---
title: PHP curl with socks5 proxy via ssh
tags:
    - PHP
    - Curl
---

Initialize a local "dynamic" application-level port forwarding via ssh:

	ssh user@host -D 10001 -v # with debug

And then create the curl session with:

	// initialize curl
	$ch = curl_init();

	// ...more options...

	// set proxy options
	curl_setopt($ch, CURLOPT_PROXYTYPE, CURLPROXY_SOCKS5);
	curl_setopt($ch, CURLOPT_PROXY, 'socks5://localhost:10001');
			