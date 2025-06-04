+++
date = 2025-06-04
description = "Resolve DNS from various nameservers and locations."
draft = false
title = "How to check DNS resolution globally"

[extra]
keywords = "DNS"
series = "Networking"
toc = true

[taxonomies]
tags = [
    "DNS",
    "Networking",
]
+++

DNS resolution converts a domain name to a IPv4 or IPv6 address. To check how a domain is being resolved globally, use the Linux tools.

## doggo

[doggo](https://github.com/mr-karan/doggo) is a command line DNS client (like `dig`). Install doggo, if not present.

```bash
brew install doggo
```

With doggo, there are two methods through which the domain can be resolved globally. One is using the public nameservers and the other is to use [globalping.io](https://globalping.io/) which is a global distributed network of probes.

### Nameservers

```bash
doggo --query example.com --type NS --ipv4 --nameserver 8.8.8.8

NAME            TYPE    CLASS   TTL     ADDRESS                 NAMESERVER 
example.com.    NS      IN      86s     a.iana-servers.net.     8.8.8.8:53
example.com.    NS      IN      86s     b.iana-servers.net.     8.8.8.8:53
```

Explanation:

- `--query`: Hostname to query the DNS records.
- `--type`: Type of the DNS Record (A, MX, NS etc).
- `--ipv4`: Use IPv4 only.
- `--nameserver`: Address of a specific nameserver to send queries to (9.9.9.9, 8.8.8.8 etc).

The same command can be used without the options too, which gives the same result:

```bash
doggo example.com NS @8.8.8.8 --ipv4

NAME            TYPE    CLASS   TTL     ADDRESS                 NAMESERVER 
example.com.    NS      IN      18688s  a.iana-servers.net.     8.8.8.8:53
example.com.    NS      IN      18688s  b.iana-servers.net.     8.8.8.8:53
```

Try with other type of record:

```bash
doggo example.com A @8.8.8.8 --ipv4

NAME            TYPE    CLASS   TTL     ADDRESS         NAMESERVER 
example.com.    A       IN      241s    96.7.128.198    8.8.8.8:53
example.com.    A       IN      241s    23.215.0.136    8.8.8.8:53
example.com.    A       IN      241s    23.192.228.84   8.8.8.8:53
example.com.    A       IN      241s    23.215.0.138    8.8.8.8:53
example.com.    A       IN      241s    23.192.228.80   8.8.8.8:53
example.com.    A       IN      241s    96.7.128.175    8.8.8.8:53
```

With `--any` option, it gets all the supported type of records:

```bash
doggo example.com --any @8.8.8.8 --ipv4

NAME            TYPE    CLASS   TTL     ADDRESS                                 NAMESERVER 
example.com.    A       IN      159s    23.215.0.136                            8.8.8.8:53
example.com.    A       IN      159s    96.7.128.198                            8.8.8.8:53
example.com.    A       IN      159s    23.192.228.80                           8.8.8.8:53
example.com.    A       IN      159s    96.7.128.175                            8.8.8.8:53
example.com.    A       IN      159s    23.192.228.84                           8.8.8.8:53
example.com.    A       IN      159s    23.215.0.138                            8.8.8.8:53
example.com.    AAAA    IN      29s     2600:1406:3a00:21::173e:2e65            8.8.8.8:53
example.com.    AAAA    IN      29s     2600:1406:bc00:53::b81e:94c8            8.8.8.8:53
example.com.    AAAA    IN      29s     2600:1406:3a00:21::173e:2e66            8.8.8.8:53
example.com.    AAAA    IN      29s     2600:1408:ec00:36::1736:7f24            8.8.8.8:53
example.com.    AAAA    IN      29s     2600:1406:bc00:53::b81e:94ce            8.8.8.8:53
example.com.    AAAA    IN      29s     2600:1408:ec00:36::1736:7f31            8.8.8.8:53
example.com.    SOA     IN      1714s   ns.icann.org.                           8.8.8.8:53
                                        noc.dns.icann.org. 2025011671                     
                                        7200 3600 1209600 3600                            
example.com.    MX      IN      2806s   0 .                                     8.8.8.8:53
example.com.    NS      IN      6403s   a.iana-servers.net.                     8.8.8.8:53
example.com.    NS      IN      6403s   b.iana-servers.net.                     8.8.8.8:53
example.com.    SOA     IN      1800s   ns.icann.org.                           8.8.8.8:53
                                        noc.dns.icann.org. 2025011671                     
                                        7200 3600 1209600 3600                            
example.com.    SOA     IN      96s     ns.icann.org.                           8.8.8.8:53
                                        noc.dns.icann.org. 2025011671                     
                                        7200 3600 1209600 3600                            
example.com.    SOA     IN      1800s   ns.icann.org.                           8.8.8.8:53
                                        noc.dns.icann.org. 2025011671                     
                                        7200 3600 1209600 3600                            
example.com.    TXT     IN      11715s  "_k2n1y4vw3qtb4skdx9e7dxt97qrmmq9"      8.8.8.8:53
example.com.    TXT     IN      11715s  "v=spf1 -all"                           8.8.8.8:53
example.com.    SOA     IN      1800s   ns.icann.org.                           8.8.8.8:53
                                        noc.dns.icann.org. 2025011671                     
                                        7200 3600 1209600 3600         
```


### Globalping

> With Globalping, there is an API limit. Make sure to use it properly otherwise it will throttle.

```bash
doggo --query example.com --type NS --ipv4 --gp-from DE

LOCATION                        NAME            TYPE    CLASS   TTL     ADDRESS                 NAMESERVER  
Falkenstein, DE, EU, Hetzner                                                                               
Online GmbH (AS24940)                                                                                      
                                example.com.    NS      IN      79810s  b.iana-servers.net.     185.12.64.2
                                example.com.    NS      IN      79810s  a.iana-servers.net.     185.12.64.2
```

Explanation:

- `--query`: Hostname to query the DNS records.
- `--type`: Type of the DNS Record (A, MX, NS etc).
- `--ipv4`: Use IPv4 only.
- `--gp-from`: Query using Globalping API from a specific location.

Try with other type of record:

```bash
doggo --query example.com --type A --ipv4 --gp-from DE

LOCATION                        NAME            TYPE    CLASS   TTL     ADDRESS         NAMESERVER 
Falkenstein, DE, EU, Hetzner                                                                      
Online GmbH (AS24940)                                                                             
                                example.com.    A       IN      117s    96.7.128.198    private   
                                example.com.    A       IN      117s    23.192.228.80   private   
                                example.com.    A       IN      117s    23.192.228.84   private   
                                example.com.    A       IN      117s    23.215.0.136    private   
                                example.com.    A       IN      117s    23.215.0.138    private   
                                example.com.    A       IN      117s    96.7.128.175    private   
```

## dig

```bash
dig @8.8.8.8 example.com

; <<>> DiG 9.10.6 <<>> @8.8.8.8 example.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 1787
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;example.com.                   IN      A

;; ANSWER SECTION:
example.com.            170     IN      A       23.192.228.84
example.com.            170     IN      A       96.7.128.175
example.com.            170     IN      A       23.192.228.80
example.com.            170     IN      A       96.7.128.198
example.com.            170     IN      A       23.215.0.138
example.com.            170     IN      A       23.215.0.136

;; Query time: 35 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Thu Jun 05 01:18:51 CEST 2025
;; MSG SIZE  rcvd: 136
```

Explanation:

- `@8.8.8.8`: Specify the DNS nameserver
- Default record type is `A`.

To get shorter response, add the `+short` option:

```bash
dig @8.8.8.8 +short example.com

23.215.0.136
96.7.128.175
23.192.228.80
23.215.0.138
23.192.228.84
96.7.128.198
```

Provide the type of record to query for:

```bash
dig @8.8.8.8 +short example.com NS

a.iana-servers.net.
b.iana-servers.net.
```
