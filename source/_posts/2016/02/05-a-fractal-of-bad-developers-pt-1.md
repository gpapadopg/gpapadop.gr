---
title: A fractal of BAD developers Pt.1
tags:
    - PHP
    - Codesod

---

Recently, we moved a customer's website to one of our servers. The site is written 
in PHP with all these things you want to avoid: Register globals, bad procedural code,
code repetitions, etc.

One of the things we had to deal with was creating the database. Because
in every php file the developers were using this code:

	$db=mysql_connect("localhost","XXX","YYY");
	mysql_select_db("ZZZ",$db);

we <strike>forced</strike> ended up creating a user `XXX` with password `YYY` on database `ZZZ`
(yeah i know, this is where sed might be useful). Joking aside, this is where
[require](http://php.net/manual/en/function.require.php) statement come in handy.
So instead of the above, you may write this:

	include "database.php";

and create a `database.php` file with the following code:

	<?php
	$db = mysql_connect("localhost","XXX","YYY");
	mysql_select_db("ZZZ",$db);

Also, notice the use of the deprecated (and removed from PHP 7)
[mysql](http://php.net/manual/en/intro.mysql.php) extension.

To be continued...