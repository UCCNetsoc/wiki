---
title: DNS
description: External and internal DNS
published: true
date: 2020-07-12T22:47:25.492Z
tags: 
editor: markdown
---

# DNS

# External DNS
Our external DNS is managed by Cloudflare. You can set new records by modifying `setup-external-dns.yml`


# Internal DNS
Our internal DNS is managed by FreeIPA. The DNS server should be the IP address of the FreeIPA server.
FreeIPA should add A records for every enrolled host (i.e their hostname).

You can add additional A records by modifying `setup-internal-dns.yml`