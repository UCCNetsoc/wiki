---
title: Level 2 Networking
description: 
published: true
date: 2021-06-03T19:42:54.523Z
tags: 
editor: markdown
---

# TLDR: Level 2 Networking

* Layer 2
	* Known as the _data link_
	* Don't worry about IP Addresses yet, it's not relevant here.

* Data is stored in frames
	* Think just arrays of bytes

* Major problem:
	* How do you know who someone is?
  * Devices need an identifier
  	* **MAC Address**
    	* Think of it like a 'hardware address'
      * Looks like this: **4e:4f:41:48:00:ef**
    * A network card (basically an ethernet jack) has a MAC address
  * No routing
  * These identifiers are stored in the frame
  	* i.e Sender MAC address and Destination MAC address
  
* L2 concerns how data is passed between adjacent network nodes/hardware
	* Data goes into switch, how does switch decide to send the data?
  * Switch doesn't know who has what MAC address (yet), so it just broadcasts the frame to everyone
  	* i.e it sends the frame out of every port in the switch
  * Anyone who recieves the frame who isn't the intended destination will just drop the frame
  	* i.e a reciever reads the frame, sees that it's MAC address isn't the destination MAC address and just ignores the frame
  * Receiver can reply and the switch will repeat the process
  * Over-time a (modern) switch can _learn_ what devices are on what ports on the switch, so it 
  	might not broadcast to everyone.
    	* Stores them in a table, can be known as MAC or CAM table
  * Remember: the switch is _not_ a router. A router only cares about IP Addresses.
  
* L2 is typically untrusted
	* Anyone can say what MAC address they are
  * Anyone can see broadcasts
  * i.e Server says I have MAC 1234, it will receive packets for MAC 1234
  
![how-a-switch-learns-mac-addresses-step-two.jpg](/sysadmin/tldr/how-a-switch-learns-mac-addresses-step-two.jpg)

The picture above shows:
	* a LAN (LOcal Area Network) of 4 devices
  * 3 computers