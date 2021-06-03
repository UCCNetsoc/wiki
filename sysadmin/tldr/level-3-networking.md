---
title: Level 3 Networking
description: 
published: true
date: 2021-06-03T20:58:43.707Z
tags: 
editor: markdown
---

# TLDR: Level 3 Networking


* Layer 3
	* Known as the _Network Link_
  
* Major problem:
	* How do you communicate past a LAN and past hardware?
  * Devices need an identifier
  	* **IP Address**
      * It's just a number
      * IPv4 => 32bits
      	* Can have a representation of 4 octets with range 0-254
        	* Different IP address ranges have different purposes and may be reserved
        	* Example:
          		1.1.1.1 (number: 16843009)
              192.168.1.254 (number: 3232236030)
              85.22.33.44 (number: 1427513644)
      * IPv6 => 128bits
      * 'Public' space
      	* Assigned to companies and ISPs, they "own" these IPs and their devices can use them
      * 'Private' space
      	* Can be used inside internal networks and should only be routed internally inside a companies
        	/ your home network
        * Examples:
        	10.0.0.2
          192.168.1.1
          172.16.2.7

![ipv4-octet.png](/sysadmin/tldr/ipv4-octet.png)

* How is Level 3 sent?
  * As a 'packet'
	* Concatenated onto Level 2 frames.
  
![osi.jpg](/sysadmin/tldr/osi.jpg)

* Major problem?
	* Representing an IP network range (e.g. 192.168.1.1 to 192.168.1.255) in an efficient fashion
  		* Known as a **network subnet**
  * Use CIDR: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
  * Store an IP address as a number and a **subnet mask** as a number
  	* A subnet mask is a number that is a power of 2
    * It's presented as what power of 2 that is using CIDR notation, e.g. /16, /32 or using subnet mask notation
    	* Example:
      	/24 => 255.255.255.0
        /8 => 255.0.0.0
  * The subnet mask represents the bits of an IP that need to be **masked** or set to 0
  * We can represent 192.168.1.1 to 192.168.1.255 by:
  	* Using the IP address 192.168.1._(any number)_
    * Using the subnet mask 255.255.255.0
    * e.g. the octets are 'masked' off using a binary AND, so the address can be visualized 
    	as 192.168.1.x, i.e any value for x
    * Written in CIDR notation => 192.168.1.0/24
    
![ipaddresses.png](/sysadmin/tldr/ipaddresses.png)
![cidr-table.png](/sysadmin/tldr/cidr-table.png)

* Example IP spaces in CIDR:
	* Some private space:
  	10.0.0.0/8 => 10.0.0.0 to 10.255.255.254
  * A small internal subnet:
  	10.0.20.0/24	=> 10.0.20.0 to 10.0.20.254
    10.40.0.0/16  => 10.40.0.0 to 10.40.255.254
  * The entire internet in IPv4:
  	0.0.0.0/0 => 0.0.0.0 to 255.255.255.254
  * A very small subnet:
  	10.1.0.0/29 -> 10.1.0.0 to 10.1.0.7
  * A single IP:
  	208.130.29.33/32
    
## Level 2 to Level 3 Boundary

* Devices self-report their IP addresses again
* How do you figure out what IP Addresses are on a LAN from Level 2?
	* **Address Resolution Protocol**
  * Devices maintain internal tables that map MAC addresses to IP addresses
  ![2021-06-03_20-53.png](/sysadmin/tldr/2021-06-03_20-53.png)
  
  * ARP is how they figure out who has what address by using the broadcast MAC address
  	* Device needs to know if ayone has 192.168.1.5. Will send 'Who has 192.168.1.5?' to broadcast MAC
    * Switches will broadcast (i.e send to every port on that LAN) a request on L2 
    	* e.g. Who has 192.168.1.5?
    * Device with that IP will respond via L2 to the sender MAC
    
    ![2020-05-15_19-26-21.png](/sysadmin/tldr/2020-05-15_19-26-21.png)
    
## Routing

* Every computer and router has a **routing table**
