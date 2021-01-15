---
title: DNS
description: External and internal DNS
published: true
date: 2020-11-18T11:48:12.461Z
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

# Internal DNS (only on `infra` VLAN)
Internal DNS is DNS lookups served whilst inside UCC Netsoc (typically inside one of our VMs)

Our internal DNS is managed by FreeIPA. The DNS server in use should be the IP address of the FreeIPA server.
FreeIPA should add A records for every enrolled host (i.e their hostname).

You can add additional records by modifying:

https://github.com/UCCNetsoc/NaC/blob/master/provision-infra-auth-internal-dns.yml