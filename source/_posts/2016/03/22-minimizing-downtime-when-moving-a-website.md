---
title: Minimizing downtime when moving a website
tags:
    - Apache
    - Reverse Proxy
---

Situation: you move a website to another server but the dns changes
propagates slowly and this causes the effect that some visitors 
sees the new site and some others the old. 

One way to cope with this is by setting a reverse proxy. The purpose 
of the proxy is to direct all HTTP traffic to another server.
In apache, this can be accomplished with these directives:

	# A.B.C.D is the server IP that gets the traffic
	ProxyPreserveHost On
	ProxyPass / http://A.B.C.D/
	ProxyPassReverse / http://A.B.C.D/

Don't forget to install and enable the `mod_proxy` mod.