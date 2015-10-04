---
title: Why I Stopped Using Blue Mail (Type Mail)
tags:
    - Android

---

[David Sklar](http://www.sklar.com/) explains [why he stopped using Blue Mail](http://www.sklar.com/2014/10/14/blue-mail/)
on his android phone:

> Fast forward a few weeks when I was noodling around and wondering what
sort of network traffic my phone was doing when I did routine tasks.
I ran a tcpdump on my home router to capture some traffic and loaded
it up into Wireshark to investigate.

> Most of the traffic looked familiar: IMAP and SMTP to my mail servers,
HTTP to some web hosts I browsed. **But there was a connection to port 10101
on an address that resolved to an AWS host. The payload was garbled – probably
TLS. What was it?**
