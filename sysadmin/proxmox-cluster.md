---
title: Proxmox Cluster
description: 
published: true
date: 2020-11-13T08:27:10.509Z
tags: 
editor: markdown
---

# Proxmox

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
