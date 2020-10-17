---
title: webN
description: User web services VM
published: true
date: 2020-10-17T11:43:43.778Z
tags: 
editor: undefined
---

# Playbooks
[Creation Playbook](https://github.com/UCCNetsoc/NaC/blob/master/create-webN.yml)
[Provision Playbook](https://github.com/UCCNetsoc/NaC/blob/master/provision-webN.yml)

IP addresses are defined as usual in [`vars/network.yml`](https://raw.githubusercontent.com/UCCNetsoc/NaC/master/vars/network.yml)

Multiple instances of this VM are created for each Proxmox host

# Summary

This VM runs Nginx Unit & the netsocadmin websites API

The websites API crawls the user home directories mounted by NFS and validates each user website.
It then configures Nginx Unit to serve each of the users websites based on their requested config

# Ports
80 for Nginx Unit
