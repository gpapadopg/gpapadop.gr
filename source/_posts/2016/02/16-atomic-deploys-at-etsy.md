---
title: Atomic deploys at Etsy
tags:
    - PHP
    - Web
---


[Rasmus Lerdorf](http://toys.lerdorf.com) explains the logic behind
[atomic deploys at Etsy](https://codeascraft.com/2013/07/01/atomic-deploys-at-etsy/):

> A key part of Continuous Integration is being able to deploy quickly, safely and with minimal impact to production traffic. Sites use various deploy automation tools like Capistrano, Fabric and a large number of homegrown rsync-based ones. At Etsy we use a tool we built and open-sourced called Deployinator.