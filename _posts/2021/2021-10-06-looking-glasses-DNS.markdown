---
layout: "post"
title: "Looking Glasses DNS"
date: "2021-10-06 22:31"
---
After the facebook issues yesterday, SANS ISC published a nice post regarding "[looking glasses](https://isc.sans.edu/forums/diary/Looking+Glasses+Debugging+Network+Connectivity+Issues/27904/)", websites allowing you to query the routing table of various routers.

[http://www.bgplookingglass.com](http://www.bgplookingglass.com) lists public-looking glasses with the above mentioned post using [https://lookingglass.centurylink.com](https://lookingglass.centurylink.com).

You need a current IP adress for the host, (whois.arin.net, "whois"). Then it was the "dig +short 83.71.203.159.origin.asn.cymru.com TXT" to get correct network segment an all that. Check out the blogpost and try yourself.
