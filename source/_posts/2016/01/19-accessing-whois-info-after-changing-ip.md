---
title: Accessing Whois info after changing IP
tags:
    - DNS

---

The problem: you changed IP/nameservers for a site and after DNS changes propagated, 
you found out you need some information from the old site.

If you are lucky (and quick) enough, you can work around this problem by visiting
[whois.domaintools.com](http://whois.domaintools.com/) and searching for the site you want.
Look for the *IP Address* (eg A.B.C.D), make sure it's not the same as the new site and put 
it in your `/etc/hosts` file like this:

    # the format is <IP> <host>
    A.B.C.D  my.site

Now you can visit the old site (eg my.site) from your browser, if it's still available 
from the old host.
