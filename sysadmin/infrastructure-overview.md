---
title: Infrastructure Overview
description: 
published: true
date: 2020-07-10T01:39:04.543Z
tags: 
editor: markdown
---



# Networking Overview
## VLANs

```

# vlan tags
vlan:
  wan:    10  # Uplink, connected directly to patch panel
  vmhost: 20  # Used for management traffic and proxmox clustering, has NAT to WAN
  infra:  30  # Used for infra VMs, can route to user and vmhost, has NAT to WAN
  user:   40  # User VM traffic, includes the User ssh server and user databases
  extern: 60  # VRRP stuff
  router: 70  # Used for vyos clustering + failover
  oob:    80  # OOB, connected to CIX OOBnet and OOB ports on machines
  mgmt:   90  #
  dhcp:  100  # DHCP Vlan, used to offer IP leases to building Packer templates

# 10.0.x.0/24
subnets:
  wan:    10  # Uplink, connected directly to patch panel
  vmhost: 20  # Used for management traffic and proxmox clustering, has NAT to WAN
  infra:  30  # Used for infra VMs, can route to user and vmhost, has NAT to WAN
  user:   40  # User VM traffic, includes the User ssh server and user databases
  extern: 60  # VRRP stuff
  router: 70  # Used for vyos clustering + failover
  oob:    80  # OOB, connected to CIX OOBnet and OOB ports on machines
  mgmt:   90  #
  dhcp:  100  # DHCP Vlan, used to offer IP leases to building Packer templates

gateway4:
  infra: "10.0.{{ subnets.infra }}.1"
  user: "10.0.{{ subnets.user }}.1"
```

### VLAN x has subnet 10.0.x.0/24


All VM host machines have a trunk connection, all VLANs. Won’t use WAN, though.
VM hosts with router have two extra connections: a WAN access link and a trunk link. Both are passed through directly to VM.
All machines have an OOB connection.


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


# VM Definitions Overview


**IP allocations for these VMs can be found in `vars/network.yml` and are subject to change**

* auth.vm.netsoc.co
	* Hosts a FreeIPA server
      * Handles DNS for the entire cluster
        * *.vm.netsoc.co
      * Kerberos Domain Controller & CA
      * Lets us define HBAC on a per user-group basis
      * Amazing tooling
      * LDAP for user accounts
      * Great web UI and great API for creating new users
 	* Hosts Keycloak
  	* Keycloak federates authentication to FreeIPA's LDAP server
    	* This lets us a run OAuth2 and OpenID Connect provider
      * i.e no more need to bind LDAP on most of our services anymore, just use our OAuth/OIDC.
      * Everything now uses JWTs! So we can split the next netsocadmin into many different services without needing to contact an authentication server
      	* Databases API service
        * Websites API service
        * Backups API service
        * Can "run" everything without the need of a web interface like we did in netsocadmin2
        	* Could provide a CLI alongside web ui that calls the same REST endpoints

**Every single other VM bar the router should be enrolled in the FreeIPA server**

* nfs.vm.netsoc.co
  * Provides an authenticated Kerberos NFS server
  * Stores home directories and docker data
  * ZFS is used on the data disk, `zfs-auto-snapshot` is setup to frequently snapshot every pool
  * A script exists that periodically should scan FreeIPA and create zfs directories for users
  	* We do this instead of sshing into the server to prevent having to give an SSH key to the account API

* databases.vm.netsoc.co
  * Hosts MySQL & Postgres databases for infra on NIC 1
  * Hosts MySQL databsae for users on NIC 2
  * DNS record is set in FreeIPA for `postgres-infra.databases.vm.netsoc.co`

* managerN.vm.netsoc.co, workerN.vm.netsoc.co
  * Docker swarm managers and workers
  * Has NFSv4 mounted /nfs/docker/*
  * Hosts the following containers:
    * Traefik (this reverse proxies the **majority** of our web services. It is **vital**)
    	* Reverse proxies FreeIPA&Keycloak
      * {discord,esports}.netsoc.co
      * netsoc.co
      * blog.netsoc.co
      * wiki.netsoc.co

* router.vm.netsoc.co
  * The VyOS router
  * This is the only VM that should bind public IPs (when migrating from our old infra is done)
  * Notable mappings:
    * 80,443 on the feynman IP maps to manager0.vm.netsoc.co and manager1.vm.netsoc.co (where traefik is hosted)

* portal.vm.netsoc.co
  * The user server (to replace leela)
  * Contains a neat SSH banner
  * NFSv4 mounted home dirs /home/users
  
* games.vm.netsoc.co (planned)
	* Minecraft + pterodactyl worker server host
  
* webN.vm.netsoc.co
	* Nginx Unit servers to run user websites/applications