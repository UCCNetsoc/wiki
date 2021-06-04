---
title: Blameless Postmortems
description: Sometimes things go bang
published: true
date: 2021-04-15T15:48:22.283Z
tags: 
editor: undefined
---

# 2020-2021


## 2020-07-20 - Overprovisioned disks on Proxmox host `lovelace`

### The issue
The `local-lvm` storage for the Proxmox host `lovelace` ran out. This caused
massive failures taking every single VM offline and locking the machine out on SSH.

We also noticed errors in one of the newly cloned VMs showed this warning (this is seperate albeit similar issue):

``` 
WARNING: You have not turned on protection against thin pools running out of space.
  WARNING: Set activation/thin_pool_autoextend_threshold below 100 to trigger automatic extension of thin pools before they get full.
  Logical volume "vm-2153420-cloudinit" created.
  WARNING: Sum of all thin volume sizes (<747.04 GiB) exceeds the size of thin pool pve/data (<378.36 GiB).
create full clone of drive virtio0 (local-lvm:base-557264-disk-0)
  WARNING: You have not turned on protection against thin pools running out of space.
  WARNING: Set activation/thin_pool_autoextend_threshold below 100 to trigger automatic extension of thin pools before they get full.
  Logical volume "vm-2153420-disk-0" created.
```

### The fix

We needed to add more space to the LVM logical volume `data`

