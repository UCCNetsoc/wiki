---
title: Networking Overview
description: 
published: true
date: 2020-07-13T21:47:47.524Z
tags: 
editor: undefined
---

# Networking Overview

## IP inventory
We posess the following IPs:

CIX-allocated:
```
84.39.234.49/29

Network:   84.39.234.48/29       01010100.00100111.11101010.00110 000 (Class A)
Broadcast: 84.39.234.55          01010100.00100111.11101010.00110 111
HostMin:   84.39.234.49          01010100.00100111.11101010.00110 001
HostMax:   84.39.234.54          01010100.00100111.11101010.00110 110

Hosts/Net: 6                     
```

UCC-allocated:
```
143.239.87.40
143.239.87.41
```

## Switch & Router

All Proxmox machines have a trunk connection (contains all vlans) that is not connected to WAN.
The Proxmox machine which runs the virtual router has two extra connections: a WAN access link and a trunk link. Both are passed through directly to the VM.

All machines have an OOB connection.

## VLAN setup

### Important: VLAN x has subnet 10.0.x.0/24

[See `vlan` and `subnet` here](https://github.com/UCCNetsoc/NaC/blob/master/vars/network.yml)

### OOB/rack assignments (for CIX OOB)

* bertha
	* 10.253.97.132

* boole
	* 10.253.97.137

* leela
	* 10.253.97.140

* feynman
  * 10.253.97.142

* lovelace
  * 10.253.97.144

* switch (pants)
  * 10.253.97.148
  
From [CloudCIX rocky](https://github.com/CloudCIX/rocky)
```
Rocky is designed to operate in an out of band (OOB) network, serarated from other CloudCIX networks. Rocky's purpose is to facilitate monitoring, testing, debug and recovery. By convention, Rocky uses OOB addresses that are IPV4, RFC1918 addresses in the form 10.S.R.U/16 where...

S represents the Site Number. If you are taking a CloudCIX support contract you will be informed of the S octet to use. Otherwise, you can choose any number you wish between 0 and 255. Each Support Number represents a VPN tunnel used by the CloudCIX support centre to reach the site.
R represents the Rack. It can be any number from 1 to 255. If you have a multi-rack SRXPod then it is recommended to number them from 1 upwards sequentially. If you have a site with multiple SRXPods then different R numbers must be used. The R number must be unique within a Site.
U represents the U location within the Rack of the device.
```

# VM IP allocations
 
[See `interfaces` here](https://github.com/UCCNetsoc/NaC/blob/master/vars/network.yml)