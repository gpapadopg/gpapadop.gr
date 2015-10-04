---
title: Magento's memory limit on CLI
tags:
    - PHP
    - Magento

---

Recently, I noticed a command line script built for a Magento store,
terminated with out of memory errors:

    PHP Fatal error:  Allowed memory size of 268435456 bytes exhausted...

As it turns out, Magento is reading the `.htaccess` file **even if
running from command line** and applies any php configuration found to the
script environment.

In order to fix this, find the line

    php_value memory_limit 256M

and get rid of it.
