---
title: System Overview
description: 
published: true
date: 2020-11-13T07:16:35.270Z
tags: 
editor: markdown
---

# System Overview

Some networking configuration is managed by Ansible, others is managed manually and is labeled as such

## IP subnet inventory

CIX allocated:
```
84.39.234.49/29

Network:   84.39.234.48/29       01010100.00100111.11101010.00110 000 (Class A)
Broadcast: 84.39.234.55          01010100.00100111.11101010.00110 111
HostMin:   84.39.234.49          01010100.00100111.11101010.00110 001
HostMax:   84.39.234.54          01010100.00100111.11101010.00110 110

Hosts/Net: 6                     
```

UCC allocated:
```
143.239.87.40
143.239.87.41
```

## Switch hardware

* Cisco WS-C3560E-24TD

```
Cisco IOS Software, C3560E Software (C3560E-UNIVERSALK9-M), Version 12.2(46)SE, RELEASE SOFTWARE (fc2)
Copyright (c) 1986-2008 by Cisco Systems, Inc.
Compiled Thu 21-Aug-08 15:09 by nachen
Image text-base: 0x00003000, data-base: 0x02000000

ROM: Bootstrap program is C3560E boot loader
BOOTLDR: C3560E Boot Loader (C3560E-HBOOT-M) Version 12.2(44r)SE3, RELEASE SOFTWARE (fc3)

pants uptime is 41 weeks, 5 days, 20 hours, 19 minutes
System returned to ROM by power-on
System image file is "flash:/c3560e-universalk9-mz.122-46.SE/c3560e-universalk9-mz.122-46.SE.bin"

If you require further assistance please contact us by sending email to
export@cisco.com.

License Level: ipbase
License Type: Permanent
Next reload license Level: ipbase

cisco WS-C3560E-24TD (PowerPC405) processor (revision G0) with 122880K/8184K bytes of memory.
Processor board ID FDO1307R0W9
Last reset from power-on
2 Virtual Ethernet interfaces
1 FastEthernet interface
28 Gigabit Ethernet interfaces
2 Ten Gigabit Ethernet interfaces

512K bytes of flash-simulated non-volatile configuration memory.
Base ethernet MAC Address       : 00:24:C4:C1:FE:00
Motherboard assembly number     : 73-11173-13
Motherboard serial number       : FDO13070UNS
Model revision number           : G0
Motherboard revision number     : A0
Model number                    : WS-C3560E-24TD-S
Daughterboard assembly number   : 800-29737-01
Daughterboard serial number     : FDO1307019P
System serial number            : FDO1307R0W9
Top Assembly Part Number        : 800-28914-01
Top Assembly Revision Number    : C0
Version ID                      : V03
CLEI Code Number                : COMSA00ARB
Hardware Board Revision Number  : 0x00


Switch Ports Model              SW Version            SW Image                 
------ ----- -----              ----------            ----------               
*    1 30    WS-C3560E-24TD     12.2(46)SE            C3560E-UNIVERSALK9-M 
```

### (MANUAL) Configuration

You can telnet into the switch on 10.0.90.2 or use SSH (with an insecure cipher spec) from any server which has a bridge that tags to VLAN 90. See VLAN config below

#### VLAN configuration

```
pants#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/2, Gi0/4, Gi0/5, Gi0/6, Gi0/7, Gi0/8, Gi0/9, Gi0/10, Gi0/11, Gi0/12, Gi0/15, Gi0/16, Gi0/17
                                                Gi0/18, Gi0/25, Gi0/26, Gi0/27, Gi0/28
10   wan                              active    Gi0/1, Gi0/19, Gi0/20, Gi0/21, Gi0/22, Gi0/23, Gi0/24
20   vmhost                           active    
30   infra                            active    
40   cloud                            active    
50   k8s                              active    
70   router                           active    
80   oob                              active    Gi0/3
90   mgmt                             active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0   
10   enet  100010     1500  -      -      -        -    -        0      0   
20   enet  100020     1500  -      -      -        -    -        0      0   
30   enet  100030     1500  -      -      -        -    -        0      0   
40   enet  100040     1500  -      -      -        -    -        0      0   
50   enet  100050     1500  -      -      -        -    -        0      0   
70   enet  100070     1500  -      -      -        -    -        0      0   
80   enet  100080     1500  -      -      -        -    -        0      0   
90   enet  100090     1500  -      -      -        -    -        0      0   
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

Remote SPAN VLANs
------------------------------------------------------------------------------


Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------
```

All servers have a trunk connection (contains all vlans) that is not connected to WAN.

The Proxmox machine which runs the virtual router has an extra connection: a WAN access link. Both are passed through directly to the VM.

## Router

We use the virtual router VyOS running in a VM. It is attached directly to the vmbr0 bridge that receives all trunk traffic, so it can see all VLANs. The virtual NIC attached to the VM is not tagged
https://www.vyos.io/


### (MANUAL) Configuration
VyOS is configured to provide default gateway on each VLAN so we can route packets between VLANs using firewall zones.

Note that not every VLAN has a similar subnet mask

i.e the infra vlan (id 30) is a `10.0.30.0/24` because we don't expect to have many infra VMs (0-255)
i.e but the cloud vlan (id 40) is a `10.40.0.0/16` because we expect to have many allocations over time (0-65k)

[See `vlan` and `subnet` here for more info](https://github.com/UCCNetsoc/NaC/blob/master/vars/network.yml#L14)

## (ANSIBLE) VM IP allocations
 
[See `interfaces` here](https://github.com/UCCNetsoc/NaC/blob/master/vars/network.yml)

##

### Out of bound rack assignments (for CIX OOB)

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