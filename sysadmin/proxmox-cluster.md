---
title: Proxmox Cluster
description: 
published: true
date: 2020-11-13T10:36:39.055Z
tags: 
editor: markdown
---

We run [Proxmox VE](https://pve.proxmox.com/), a system that abstracts KVM virtual machines and LXC containers into a common set of powerful tools.

It's also got a nice web UI and _really_ nice API. Think of it like an open-source ESXi/vSphere

Our Proxmox cluster looks like this:

* feynman
  * Designated as our 'control' server
  	* This means that feynman is our 'point of entry' for getting into our internal network from the public internet
    	* i.e SysAdmins ssh into it and use the web UI from here
    * We run scripts and tools to automate deployment and configuration from this host
    	* This means we ssh to all our other Proxmox hosts and run tasks from our control host
  * You can ssh into it by running `ssh <username>@control.netsoc.co -p2222`
  * You can also visit the web UI: [https://control.netsoc.co:8006](https://control.netsoc.co:8006)

* lovelace
	* Just a generic Proxmox host

Servers that still need to become Proxmox cluster hosts (these still run historical/legacy setups):

* bigbertha
* leela
* boole


We currently do _not_ plan on using Ceph (distributed storage) on our cluster. This is because Ceph requires a minimum quorum of 3 servers to operate redundantly. We may use it in the future.


# VM Storage

* Proxmox stores data in 'pools'
* A pool can either be a directory or a block device
	* Block device => data is written directly to the hard drive
  * File => data is written a file in the file system
  
* A file pool might just be the 'Dir' pool which is a directory on the host
	* 'local' in Proxmox, which corresponds to /var/lib/vz
  	* Still stored on an LVM partition
    
* A block pool might be LVM
	* LVM is Logical Volume Manager
  * It lets you create what appears to be 1 single 'logical' volume/partition but it could be spread across 2-3 physical disks
  * You can combine multiple Physical Volumes (i.e 2-3 hard drives/ssds) into a Volume Group (VG)
  * You can then subdivide the Volume Group (VG) into Logical Volumes (LV) which appear as disks of the size you specify
  	* See: https://wiki.archlinux.org/index.php/LVM
    * See this on thin provisioning: 
    	* https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/assembly_thinly-provisioned-logical-volumes_configuring-and-managing-logical-volumes
  * Proxmoxes LVM pool (named 'local-lvm') creates a Logical Volume for each VM disk
  
  

## RAID (TODO - planned)

### feynman

4x 3.5inch LFF

### lovelace

4x 3.5inch LFF

### leela

6x 3.5inch LFF

6x935GB RAID 10 mdadm
