---
title: Getting a database backup via php
tags:
    - Code Snippets
    - PHP
    - MySQL

---
    <?php
    //
    // Download a gzipped mysql dump from a remote host
    //
    // Invoke it via curl with:
    //  curl http://remote.site/path/to/script.php -o /path/to/dump.sql.gz
    //

    // replace with your own values
    $username = escapeshellarg("YOUR DATABASE USER");
    $password = escapeshellarg("YOUR DATABASE PASSWORD");
    $database = escapeshellarg("YOUR DATABASE NAME");

    // dump it!
    passthru("mysqldump -u$username -p$password $database | gzip");
