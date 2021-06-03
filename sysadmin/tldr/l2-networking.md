---
title: Level 2 Networking
description: 
published: true
date: 2021-06-03T20:11:50.123Z
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
  
![l2-frame.jpg](/sysadmin/tldr/l2-frame.jpg)
  
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
  
  * a LAN (Local Area Network) of 4 devices
  * 3 computers
  * Each device has a MAC address
  
## Virtual LANs

* Think of a Virtual LAN as dividing up the ports on a switch into many different LANs
* If you wanted to put ComputerA and ComputerC on it's own LAN you could buy a new switch and have no link between switches
* A Virtual LAN allows you to do this on a single switch
* Ports on the switch can be assigned a **VLAN ID**
* When frames come into the switch, they will only be broadcast and transmitted on the same VLAN
	* ComputerA and ComputerC can be put on their own vlan and **won't be able** to communicate with ComputerB by sending frames with their MAC addresses
  	* Unless you involve a router (Level 3, with IP Addresses) which can move traffic between VLANs
* Computers in a VLAN don't know they're in the VLAN, they just see it as their LAN
	* VLAN ID isn't set

![2021-06-03_19-59.png](/sysadmin/tldr/2021-06-03_19-59.png)

## 'Trunked' VLANs

* What happens when you want to get 'all' traffic on some or all VLANs?
* A switch can have a **trunk** port.
* If data leaves the **trunk** port, the VLAN that frame came from is tagged onto the frame.
	* i.e the VLAN ID is set in the frame
* If data enters the **trunk** port, the VLAN id is read from that frame and then transmitted into the appropriate VLAN.

* You could connect a computer to a **trunk** port and it can send data to and from VLAN 10 and 30 for example (assuming it is configured correctly) without the need for a router.
	* The computer recieves the trunked frames from the switch and knows what VLAN traffic came from by reading the VLAN ID from the frame
  * It can then transmit trunked frames back to the switch to send data into a specific VLAN by attaching a specific VLAN id to the outgoing frame
