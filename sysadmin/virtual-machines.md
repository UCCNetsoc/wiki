---
title: Virtual Machines
description: Read before contributing to NaC
published: true
date: 2021-04-15T15:48:50.663Z
tags: 
editor: undefined
---

# Important Policies

* **All volatile data should be on a 2nd disk of the VM, not the boot disk**
  * Configure your services running on your VM to be independent of the boot disk
    * i.e if we want to restore from a backup, we can delete the VM, run the playbook to create a new VM, detach the empty 2nd disk it created and attach the disk we backed up
  * By defining a VM entirely by playbooks we can potentially save a LOT of disk space on backups

* **Do NOT put a VM on a trunk port unless it's a router**
  * Always give the VMs a NIC on vmrb0 with a tagged VLAN
  * If you want a prescence on multiple VLANs, attach multiple NICs
  * If you give the VM with a NIC with no tagging, it will recieve ALL traffic.
      * This is a huge security risk

* **Ensure critical services start on boot**
	 * Don't forget to set your containers to start on boot, it's easy to miss this
