---
title: Proxmox Cluster
description: 
published: true
date: 2020-11-13T08:29:41.321Z
tags: 
editor: markdown
---

We run [Proxmox VE](https://pve.proxmox.com/), a system that abstracts KVM virtual machines and LXC containers into a common set of powerful tools.

It's also got a nice web UI and _really_ nice API. Think of it like an open-source ESXi/vSphere

Our Proxmox cluster looks like this:

* feynman
  * Designated as our 'control' server
  	* This means that feynman is our 'point of entry' for getting into our internal network
    	* i.e SysAdmins ssh into it and use the web UI from here
    * We run scripts and tools to automate deployment and configuration from this host
    	* This means we connect to all our other Proxmox hosts and run tasks from our control host

* lovelace
	* Just a generic Proxmox host

Servers that still need to become Proxmox cluster hosts (these still run historical/legacy setups):

* bigbertha
* leela
* boole


We currently do _not_ plan on using Ceph (distributed storage) on our cluster. This is because Ceph requires a minimum quorum of 3 servers to operate redundantly. We may use it in the future.

## RAID (TODO - planned)

### feynman

4x 3.5inch LFF

### lovelace

4x 3.5inch LFF

### leela

6x 3.5inch LFF

6x935GB RAID 10 mdadm
