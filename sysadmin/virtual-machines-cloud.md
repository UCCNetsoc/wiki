---
title: Virtual Machines - Cloud
description: 
published: true
date: 2020-11-18T11:38:06.249Z
tags: 
editor: markdown
---

# Background

The Cloud VLAN contains VMs and containers created by Netsoc Cloud and our Proxy VM that forwards connections to VMs

# Virtual Machines

## proxy

`proxy` is our HTTPS/TCP/UDP reverse proxy and SSH jump host for Netsoc Cloud VPS & Containers.

`proxy` is NAT'd at the router with a full public IP

https://github.com/UCCNetsoc/NaC/blob/master/create-cloud-proxy.yml
https://github.com/UCCNetsoc/NaC/blob/master/provision-cloud-proxy.yml

`proxy` runs the following services:
* Traefik
	* Traefik is configured to listen to 80, 443 & the address range configured for port mapping in the Netsoc Cloud API
  * It dynamically pulls its configuration using the Traefik HTTP provider (i.e it requests the Traefik JSON config) from the Netsoc Cloud API
  


