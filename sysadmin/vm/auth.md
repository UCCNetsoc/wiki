---
title: auth [LDAP, DNS, KDC, Keycloak]
description: FreeIPA / Keycloak server
published: true
date: 2020-07-12T23:51:43.286Z
tags: 
editor: markdown
---


# Playbooks
[Creation Playbook](https://github.com/UCCNetsoc/NaC/blob/master/create-auth.yml)
[Provision Playbook](https://github.com/UCCNetsoc/NaC/blob/master/provision-auth.yml)

IP addresses are defined as usual in [`vars/network.yml`](https://raw.githubusercontent.com/UCCNetsoc/NaC/master/vars/network.yml)

# Summary

The most important VM in our arsenal.

* Hosts a FreeIPA server
	* This provides us with:
  	* LDAP - for storing users and managing user authentication
    * DNS - for providing us with internal DNS for `*.vm.netsoc.co`
    	* Any enrolled host is available as an A DNS record
    * Kerberos Domain Controller
    	* For creating Kerberos tickets
      * Used for authenticating users against NFS 
    * SSSD on any enrolled host
    	* Allows us to do Host-based Access Control
      * i.e give group `netsoc_sysadmin` access to every host, `netsoc_account`
* Hosts Keycloak
	* Keycloak provides unified OAuth2/OpenID Connect/Shibboleth authentication
  	* We have it configured to federate to FreeIPA (i.e it imports users from FreeIPA's LDAP on a regular basis)
    * This means we no longer need to bind LDAP (which can be a security risk) on most of our services
  * This allows us to provide SSO for any of our services
  	* If we use OpenID Connect, it can issue Json Web Tokens
    * These let us verify who a user is without having to contact a centralized authentication server
    	* Great for microservices!

# Ports
FreeIPA runs Web UI on 80,443. Other services like DNS are ran on their respective ports

Keycloak runs on 8080.
