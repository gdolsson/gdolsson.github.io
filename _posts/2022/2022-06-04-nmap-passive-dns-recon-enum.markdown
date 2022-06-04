---
layout: "post"
title: "NMAP - Passive DNS sources for recon and enumeration"
date: "2022-06-04 16:48"
---
Another nice writeup by [Rob at SANS ISC](https://isc.sans.edu/forums/diary/Using+Passive+DNS+sources+for+Reconnaissance+and+Enumeration/28596/).

Scanning a subnet by IP address works fine for host enumeration though not as well if there are hosted websites, each accessible through DNS names. IP adresses can land you at default pages and not the intended site/service. Using certificate scanning will provide DNS in CN (Common Name) and SAN (Subject Alternative Names) fields of certificate.

    nmap -p<list of ports> -Pn --open -sT <ip addresses or subnets> --script ssl-cert -oA certs.out

**-p**: list of ports
**-Pn**: Don't ping target, assume its there (because firewalls block ping)
**--open**: Only show open ports
**-sT**: Use a connect san (full TCP handshake)
**--script ssl-cert**: Collect certificate and show info
**-oA**: Define output filename for all three formats

Regarding port, these can be listed in a fair amount of ways including comma separated and more. Check out the [docs](https://nmap.org/book/man-port-specification.html) for details. Also remember from previous post, the `--resolve-all` flag to make sure you catch all hosts behind a load balancer. This will provide a lot of information, you can use grep/findstr to extract just DNS names:

    | findstr "Name report" | findstr /v "Issuer"

or 

    | grep 'Name\|report' | grep -v "Issuer"

Either from report or from stdout directly after `nmap` command. The output will look like:

```bash
Nmap scan report for <domain> (<ip>)
| ssl-cert: Subject: commonName=xxxxx.com
| Subject Alternative Name: DNS:*.a.org, DNS:*.b.edu, DNS:*.c.org, DNS:*.d.co, DNS:e.org, DNS:*.ds.org, ...
```