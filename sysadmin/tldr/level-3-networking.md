---
title: Level 3 Networking
description: 
published: true
date: 2021-06-03T20:23:49.178Z
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
   
* How is Level 3 sent?
  * As a 'packet'
	* Concatenated onto Level 2 frames.
  
![osi.jpg](/sysadmin/tldr/osi.jpg)
