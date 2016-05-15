---
title: Simulating non-SNI browsers on SSL/TLS websites

tags:
    - SNI
    - OpenSSL
---

Useful when you need to know whether the ssl/tls enabled website is accessible from 
ancient browsers and OS'es (taken from this stackoverflow [answer](http://stackoverflow.com/a/28297723/2053652)):

    openssl s_client -connect domain.com:443 
    # ...output...
    # ...proper response is something like
    Verify return code: 0 (ok)
    # ...invalid response is something like
    Verify return code: 18 (self signed certificate)



See the wikipedia [article](https://en.wikipedia.org/wiki/Server_Name_Indication) for an 
explanation about SNI.