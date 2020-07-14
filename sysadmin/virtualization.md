---
title: Virtualization
description: 
published: true
date: 2020-07-13T21:47:52.095Z
tags: 
editor: undefined
---

We run [Proxmox VE](https://pve.proxmox.com/), a system that abstracts KVM virtual machines and LXC containers into a common set of powerful tools.

It's also got a nice web UI and _really_ nice API. Think of it like an open-source ESXi/vSphere

Our setup consists of a cluster of Proxmox machines, there currently are as follows:

* feynman
  * Designated as our Ansible control server
    * Designated as such because it's a pretty terrible server
    * All Ansible playbooks that make changes must be run from here
    * Each SysAdmin has a Linux account created on the server to allow them to do development work
    * Fair warning: it's disks are slow af
    
* lovelace
	* Just a generic vm host

Servers that still need to become Proxmox cluster hosts (these still run the historical setups):

* bigbertha
* leela
* boole

We have not yet decided on what will be done to the UCC VMs.
We currently do _not_ plan on using Ceph (distributed storage) on our cluster. This is because Ceph requires a minimum quorum of 3 servers to operate redundantly. We may use it in the future
