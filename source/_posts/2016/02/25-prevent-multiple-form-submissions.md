---
title: Prevent multiple form submissions
tags:
    - Javascript
    - Web
---

Recently, a client reported a problem with a visitor seeing a server error page after submitting
a form. The weird thing was that the processing was successful. After looking around, I 
found this in apache's access log (some fields removed):

	"POST /some/path HTTP/1.1" 302 "https://..." "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.116 Safari/537.36"
	"POST /some/path HTTP/1.1" 500 "https://..." "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.116 Safari/537.36"

This is two POST requests 4 seconds apart from the same IP and browser. The first responds with redirect and the 
second with server error which is the page the visitor saw. When trying to reproduce this behaviour with chromium and
firefox, it turned out that the former submits a form multiple times if you are quick enough or the 
server takes a little bit to respond.

A quick solution to this problem is by applying this simple javascript snippet on the form markup:

	<form onsubmit="submit.disabled=true; return true;" ... >
		...
        <button type="submit" name="submit">
            Submit
        </button>
	</form>

Don't forget to set the `name` attribute on `button` element.
