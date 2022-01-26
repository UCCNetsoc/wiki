---
title: Historical Infrastructure
description: The past server setups of UCC Netsoc
published: true
date: 2021-06-04T00:06:32.493Z
tags: 
editor: markdown
dateCreated: 2020-07-09T23:50:23.371Z
---

# 2015-2020

Prior to 2020/2021, Netsoc ran a completely different setup on bare metal which consisted of 5 bare-metal servers & 2 virtual machines provided by UCC

Bare-metal machines:

* **leela**
  * 84.39.234.51
  * Our main user server that any of our members could ssh into
  * Hosted netsocadmin (v2)
  * UCC Express & Motley
* **bigbertha**
  * 84.39.234.50
  * Monitoring/some websites
* **lovelace**
  * 84.39.234.52
  * Game server hosting
* **boole** 
  * 84.39.234.54
  * CI/related
* **feynman**
  * 84.39.234.53
  * Root server for any user

These machines are hosted in a rack in Cork International eXchange (CIX).
Many previous and current Netsoc members had worked at CIX and were familiar with the data center

Our point-of-contact with CIX was Jerry McSweeney and noc@cix.ie, lovely chap

Our UCC VMs:
* **elon / netsoc1.ucc.ie**
  * 143.239.87.40
  * LDAP
  * MySQL
* **tesla / netsoc2.ucc.ie**
  * 143.239.87.41
  * I honestly can't remember, it's semi-perma borked

We needed to figure out our UCC point-of-contact

The setup consisted of a Docker swarm all of our bare metal servers.

We did not run a router, just a switch. As such each bare metal server was bound directly to their public IPs

We decided it was time to completely manage our infrastructure with Infrastructure-As-Code as it would help preserve the server setup over time. We therefore migrated a large amount of our setup to be configured using Ansible in 2019/2020. This eventually led to moving to the Proxmox solution in 2020/2021

# netsocadmin & netsocadmin2
This period featured the first iteration of [netsocadmin](https://github.com/uccnetsoc/netsocadmin). Originally written in PHP (using the Laravel framework)

TODO(ocanty) - more blurb
