---
layout: "post"
title: "NMAP - Passive DNS sources for recon and enumeration"
categories: [NMAP, Scanning]
tags: [NMAP, Scanning]
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

Regarding port, these can be listed in a fair amount of ways including comma separated and more. Check out the [docs](https://nmap.org/book/man-port-specification.html) for details. Also remember from [previous post](https://gdolsson.github.io/2022/06/04/nmap-hosts-behind-lbc.html), the `--resolve-all` flag to make sure you catch all hosts behind a load balancer. This will provide a lot of information, you can use grep/findstr to extract just DNS names:

    | findstr "Name report" | findstr /v "Issuer"

or 

    | grep 'Name\|report' | grep -v "Issuer"

Either from report or from stdout directly after `nmap` command. The output will look like:

```bash
Nmap scan report for <domain> (<ip>)
| ssl-cert: Subject: commonName=xxxxx.com
| Subject Alternative Name: DNS:*.a.org, DNS:*.b.edu, DNS:*.c.org, DNS:*.d.co, DNS:e.org, DNS:*.ds.org, ...
```

You will get the Fully Qualified Domain Name (FQDN) but likely a stack of wildcards as well. So how can we retrieve the relevant information? You can get some information using online services through API calls:

**ipinfo.io**
```bash
$ curl ipinfo.io/$1?token=<API KEY>

{
  "ip": "X.X.X.X",
  "anycast": true,
  "city": "Redwood City",
  "region": "California",
  "country": "US",
  "loc": "37.5331,-122.2486",
  "org": "Org Name Something",
  "postal": "846343",
  "timezone": "America/Los_Angeles"
}
```

**DNS Dumpster**
```bash
https://api.hackertarget.com/reverseiplookup/?q=<ip address>
```

Using the **ISC IP**, stripping out extra records
```bash
$ curl -s https://api.hackertarget.com/reverseiplookup/?q=<ip address>  | grep -v incapdns  | grep -v imperva
a.s.org
c.org
cde.ans.org
...
...
www.nans.edu
www.nans.org
```
**Circl.lu**
```bash
$ curl --insecure -u <your account>:<API KEY> -X GET "https://www.circl.lu/pdns/query/$1" -H "accept: application/json" | jq

{
  "count": 18,
  "origin": "https://www.circl.lu/pdns/",
  "time_first": 1518633608,
  "rrtype": "A",
  "rrname": "777jdxx.x.incapdns.net",
  "rdata": "<ip adress>",
  "time_last": 1539958408
}
```
Running the output through `jq` to "prettify" the output a bit.

***Shodab.io** "tell me about that host"
```bash
$ curl -k GET "https://api.shodan.io/shodan/host/$1?key=<API KEY>"
```

Produces a list of DNS names and ports (open at some point), piping this through `jq` will make it a lot more readable. Fields of immediate interest likely include "ports"

```bash
  "ports": [
    1024,
    7171,
    25,
    20000,
    8112
  ],
```

Finally **Cisco Umbrella**
```bash
curl "https://investigate.api.umbrella.com//pdns/ip/$1" -H 'Authorization: Bearer <API KEY>' -H 'Content-Type: application/json' | jq | tee $1.ip.umbrella.txt
```
If you just want the host names, pipe through grep as such: `cat $1.ip.umbrella.txt | grep rr`

Filtering out the icapdns and imperva information: `| tr -d \" | tr -d , | sed  "s/rr://g"` and you are on your way to something.

If you stumble across a DNS server in the list, you can get the domains hosted on that server:

```bash
curl https://investigate.api.umbrella.com/whois/nameservers/ns27.worldnic.com?limit=600 -H 'Authorization: Bearer <API Token goes here>' -H 'Content-Type: application/json' | jq | grep \"domain\"
```

Rob linked to a set of [scripts for DNS/PDNS recon](https://github.com/robvandenbrink/DNS-PDNS-Recon) available at GitHub