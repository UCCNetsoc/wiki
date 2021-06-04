---
title: DNS
description: External and internal DNS
published: true
date: 2021-04-15T15:48:26.252Z
tags: 
editor: undefined
---

# External DNS
External DNS is DNS lookups served to users outside of UCC Netsoc infrastructure

Our external DNS is managed by Cloudflare. You can set new records by modifying [`setup-external-dns.yml`](https://github.com/UCCNetsoc/NaC/blob/master/setup-external-dns.yml)


* `*.netsoc.co / netsoc.co` is reserved for prod
	* i.e _ci.netsoc.co_, _wiki.netsoc.co_

* `*.netsoc.dev / netsoc.dev` is dev/staging

* `netsoc.cloud`
  
* **CNAME any new subdomains to an ingress CNAME. Try not to use A records.**
	* i.e CNAME wiki.netsoc.co to web.netsoc.co (our inbound web server which will route the request)