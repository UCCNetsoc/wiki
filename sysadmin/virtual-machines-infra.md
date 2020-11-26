---
title: Virtual Machines - Infra
description: 
published: true
date: 2020-11-26T17:14:11.955Z
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

* **Internal DNS only manages the zone `infra.netsoc.co`**
	* See External DNS for `netsoc.co`

* Each VM _must_ be enrolled into the FreeIPA realm if they want to have a DNS record for their hostname.
	* All VM hostnames should end with `infra.netsoc.co`

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

## databases

`databases` hosts all of our databases. It should be placed on a node with both good CPU & disk performance

https://github.com/UCCNetsoc/NaC/blob/master/create-infra-databases.yml
https://github.com/UCCNetsoc/NaC/blob/master/provision-infra-databases.yml

* `databases` runs:
	* 

## web

`web` is our main reverse proxy web server and runs most of our web services

https://github.com/UCCNetsoc/NaC/blob/master/create-infra-web.yml
https://github.com/UCCNetsoc/NaC/blob/master/provision-infra-web.yml

* `web` runs:
	* Traefik, the main reverse proxy for all of our web-facing services:
  	* File config provider:
      * Used to reverse proxy applications that don't run on the web vm
        * FreeIPA Web UI on `ipa.netsoc.co`
        * Keycloak Web UI on `keycloak.netsoc.co`
        * Prometheus Web UI on `prometheus.netsoc.co`
      * Set up various redirect URLs:
        * `discord.netsoc.co`
        * `esports.netsoc.co`
        * `constitution.netsoc.co`
        * `mentorships.netsoc.co`
        * `tutorial.netsoc.co`
    * Docker config provider
    	* AutomaticallyS set by labels on the Docker containers 
 * Static Websites
 	* Homepage, Blog
 * Wiki
 * Discord Bot
 * CI
 	* Drone
 * Netsoc Cloud
 	* UI `netsoc.cloud`
  * API exposes `api.netsoc.cloud`
  	* Also exposes config that is picked by up by Traefik running on the Cloud Proxy VM
  * Does *not* handle reverse proxying Netsoc Cloud containers or VMs
  	* See Cloud Proxy for this
 * Loki (for collecting logs off machines/containers)
 * Grafana



https://github.com/UCCNetsoc/NaC/blob/master/create-infra-web.yml
https://github.com/UCCNetsoc/NaC/blob/master/provision-infra-web.yml

