---
layout: "post"
title: "NMAP - Hosts behind load balanced clusters"
date: "2022-06-04 16:48"
---
Another nice writeup by [Rob at SANS ISC](https://isc.sans.edu/forums/diary/Using+NMAP+to+Assess+Hosts+in+Load+Balanced+Clusters/28682/).

Apparently, nmap will only assess the first IP returned for a DNS query against hostname.

    nmap -Pn -sT -v -p80,443,8443,9443
    ...
    Other addresses for somehost.somedomain.com (not scanned): 5.6.7.8 13.12.11.10
    ...

The workaround to this issue is to use `--resolve-all` so that each IP is scanned (as the DNS name provides). Scanning IP addresses might not be equal to scanning the service, rather looking at the apache/IIS default server, hence this is the preferred method.

    nmap -Pn -sT -v -p80,443,8443,9443 --resolve-all somehost.somedomain.com

This switch is also useful with nmap scripts, especially the "SSL-" family of scripts.