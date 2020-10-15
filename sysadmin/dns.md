---
title: DNS
description: External and internal DNS
published: true
date: 2020-10-15T19:16:19.672Z
tags: 
editor: undefined
---

# External DNS
External DNS is DNS lookups served to users outside of UCC Netsoc infrastructure

Our external DNS is managed by Cloudflare. You can set new records by modifying [`setup-external-dns.yml`](https://github.com/UCCNetsoc/NaC/blob/master/setup-external-dns.yml)


* `*.netsoc.co` is reserved for user domains and user facing services.
	* i.e _ocanty.netsoc.co_, _wiki.netsoc.co_

* `*.netsoc.dev` is reserved for internal management domains and sysadmin services
	* i.e _grafana.netsoc.dev_, _ci.netsoc.dev_
  
* **CNAME any new subdomains to an ingress CNAME. Do not use A records.**
	* i.e CNAME wiki.netsoc.co to traefik.netsoc.co (our inbound web server which will route the request)

# Internal DNS
Internal DNS is DNS lookups served whilst inside UCC Netsoc (typically inside one of our VMs)

Our internal DNS is managed by FreeIPA. The DNS server in use should be the IP address of the FreeIPA server.
FreeIPA should add A records for every enrolled host (i.e their hostname).

You can add additional records by modifying [`setup-internal-dns.yml`](https://github.com/UCCNetsoc/NaC/blob/master/setup-internal-dns.yml)