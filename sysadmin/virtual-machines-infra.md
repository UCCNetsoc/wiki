---
title: Virtual Machines - Infra
description: 
published: true
date: 2020-11-14T21:08:08.159Z
tags: 
editor: markdown
---


# Background

These machines occupy the Infra VLAN, they run most of our services
The Infra VMs are intentionally built to be monolithic for the following reasons:
* Simplicity
* No need for huge scale, vertical scaling is fine as we expect only a few hundred users
* We run a Kubernetes installation on another VLAN as a SysAdmin plaything for fun if you're lookig for High Availability & clustering stuff

Most services should be configured to be ran inside Docker & Docker Compose.

# Internal DNS

* Internal DNS on the Infra VLAN is handled by the `auth` VM, records must be added/modified in `provision-auth-infra-internal-dns.yml`

# Virtual Machines

## auth

`auth` is our host & user authentication server

https://github.com/UCCNetsoc/NaC/blob/master/create-infra-auth.yml
https://github.com/UCCNetsoc/NaC/blob/master/provision-infra-auth.yml

* `auth` runs the following services
	* [FreeIPA](https://www.freeipa.org/page/Main_Page) server
  	* We expose the following ports on auths IP to the FreeIPA container
    	* 80 - Web UI
      * 443 - Web UI
      * 389 - LDAP
      * 636 - Secure LDAP
      * 88 - Kerberos
      * 464 - Kpasswd
      * 749 - Kpasswd
      * 123 - NTP
      * 53 - DNS
    * Provides DNS for any centrally enrolled host
     	* When auth is first created, it's DNS is set to public nameservers (Cloudflare or Google DNS)
      * After FreeIPA is installed, we set them to point at itself
      * FreeIPA will forward any request that doesn't point in the realm FreeIPA is configure to manage `INFRA.NETSOC.CO` to an external DNS server like Cloudflare
      * All other infra boxes use the `auth` VM for their DNS
  	* Provides a user store in LDAP for all our users
  * [Keycloak](https://www.keycloak.org/)
    * We expose the following ports on auths IP to the Keycloak container
    	* 8443 - Web UI
  	* Keycloak lets us provide SSO to any user we store in FreeIPA's LDAP server
    	* It can speak SAML, OAuth & OpenID Connect
      * This means that any user signed up to Netsoc Services we can federate them to login through one location. One account, as many services as they want
      * It also means in the future we could forgo Netsoc accounts altogether and federate directly to `*@umail.ucc.ie` by using Google/Microsoft OAuth federation
    * We use a [custom build of Keycloak with our theme injected](http://github.com/UCCNetsoc/keycloak)
    * We try to keep Keycloak stateless, so if you modify the configuration in Keycloak's web UI you will need to commit it back into NaC
    	* You can do this by running the `export-keycloak-freeipa-realm.yml` playbook and commiting the changed file
  
## web

`web` is our main reverse proxy web server and runs most of our web services

* `web` runs:
	* 


https://github.com/UCCNetsoc/NaC/blob/master/create-infra-web.yml
https://github.com/UCCNetsoc/NaC/blob/master/provision-infra-web.yml
