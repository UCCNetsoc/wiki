---
title: DNS
description: External and internal DNS
published: true
date: 2020-07-12T23:28:07.820Z
tags: 
editor: markdown
---


# External DNS
External DNS is DNS lookups served to users outside of UCC Netsoc infrastructure

Our external DNS is managed by Cloudflare. You can set new records by modifying `setup-external-dns.yml`


# Internal DNS
Internal DNS is DNS lookups served whilst inside UCC Netsoc (typically inside one of our VMs)

Our internal DNS is managed by FreeIPA. The DNS server in use should be the IP address of the FreeIPA server.
FreeIPA should add A records for every enrolled host (i.e their hostname).

You can add additional A records by modifying `setup-internal-dns.yml`