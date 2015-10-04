---
title: PHP developers, stop doing this!
draft: true
tags:
    - PHP

---

Recently, we moved a customer's website to one of our servers. The site is written 
in PHP with all these things you want to avoid: Register globals, bad procedural code,
code repetitions, etc.

One of the things we had to deal with was creating the database. Because
in every php file the developers were using this code:

	$db=mysql_connect("localhost","XXX","YYY");
	mysql_select_db("ZZZ",$db);

we <strike>forced</strike> ended up creating a user `XXX` with password `YYY` on database `ZZZ`
(yeah i know, this is where sed might be useful). Joking aside, **this is** where
[require](http://php.net/manual/en/function.require.php) statement come in handy.
So instead of the above, you may replace it with

	include "database.php";

and create a `database.php` file with the following:

	<?php
	$db = mysql_connect("localhost","XXX","YYY");
	mysql_select_db("ZZZ",$db);

After populating the database with data and putting the files
in place, we took another shock. The developers have developed the site with 
register_globals on, so instead of something like:

	$id = $_GET["id"];

every variable was defined in place and used like:

	$result1=mysql_query("select * from TABLE where id='$id'");

Yes, really! This `$id` may be from session, from cookie, from POST, from GET, 
from Mars, you name it!

And here is the second shock: Every sql statement full of SQL Injection 
vulnerabilities. See above again. And see it again. And see another one:

	$sn=session_id();
	$rx=mysql_query("insert into SESSION_TABLE(sessid, id,mtime) values ('$sn','$id',now())");

I know, it's 2015 and you don't believe me, so this is the full snippet:

	<?php
	$db=mysql_connect("localhost","XXX","YYY");
	mysql_select_db("ZZZ",$db);
	if(session_id() == "")
	{
	session_start();
	}
	$sn=session_id();
	$rx=mysql_query("insert into SESSION_TABLE(sessid, id,mtime) values ('$sn','$id',now())");
	$langid = 2;
	$result1=mysql_query("select * from TABLE where id='$id'");
	$myrow1 = mysql_fetch_array($result1);

Did I mention about the lack of code formatting? Or that we were running PHP 5.4?


OK let's move on. Next problem in list was the fact that we couldn't see
any other page properly. At least on pages with query parameters in URL.
Did I mention we were running PHP 5.4? Yep, and this is when
[register_globals](http://php.net/manual/en/security.globals.php) finally removed from PHP 
(to stop people doing things like this).

In order for the site to be functional, we have to apply the following 
snippet from [http://php.net/manual/en/faq.misc.php#faq.misc.registerglobals](http://php.net/manual/en/faq.misc.php#faq.misc.registerglobals):


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
	?>

via the following directive:

	auto_prepend_file = "/path/to/the/above/file.php"

## Bonus: Login page

This site have an administration interface.
This is the code snippet of the authentication logic:

	<?php
	$db=mysql_connect("localhost","XXX","YYY");
	mysql_select_db("ZZZ",$db);
	if (($name != "") && ($passwd != ""))
	        {
	        $r1=mysql_query("select * FROM TABLE WHERE user='$name' AND pass='$passwd'");
	        if ($myrowx = mysql_fetch_array($r1))
	                {
	                setcookie("login",$myrowx['id']);
	                $login = $myrowx['id'];
	                }
	        else
	                $login = 0;
	        }
	if ($logout == 1)
	        {
	        setcookie("login");
	        $login = 0;
	        }
	$r2=mysql_query("select * FROM TABLE WHERE id='$login'");
	if ($myrowq = mysql_fetch_array($r2))
	        $authlev = $myrowq["authlevel"];
	else
	        $authlev = 0;

Raw and uncut (well I actually edited some names). Can you spot any problem? Can you guess how to
bypass authentication? Try either the `login=1` parameter in url, or supply the `' OR 1 OR '` 
(with the quotes) as user/pass.