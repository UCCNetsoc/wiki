---
title: Netsoc Cloud
description: 
published: true
date: 2020-11-27T02:39:26.673Z
tags: 
editor: markdown
---

# Architecture

This page contains brief notes that explain things that may be unclear about how Netsoc Cloud works

## API

* Sign-up/account system
	* Account verification
  * Password reset functionality
  * Accounts stored in FreeIPA
  	* Done via the FreeIPA API
  
* VPS / Container instance system
	* Built on Proxmox
  	* Uses the Proxmox API + SSH
	* Containers use Proxmox LXC templates
  * VPS use boot disks for templates
  * Emails sent via SendGrid

* Reverse proxy into instance
	* Users can map external ports onto their instance
  	* Allows for any TCP/UDP traffic
  * Users can map domains onto their instance

### Authentication

Authentication speaks OpenID Connect (OIDC). 
Basically, on the API we expect the `Authorization` header to be set to a JWT that fits the OIDC format. We expect the `email` scope, then look the user up in FreeIPA to get their details

### Documentation

Visit `api.netsoc.cloud/docs`

### Configuration

Config models and default values are defined in `https://github.com/UCCNetsoc/cloud/blob/master/api/v1/models/config.py`

Config is defined in yaml and mounted in the Docker container. [Sample](https://github.com/UCCNetsoc/cloud/blob/master/config.sample.yml)

### Template System

Template metadata needs to be defined in the configuration before they will appear in the UI

TODO(jac) document

### Instance System

TODO(ocanty) document

## Frontend

The frontend is written in Vue, using Vuetify.
To get the OIDC JWT to send to the API, we use [oidc-client](https://github.com/IdentityModel/oidc-client-js), this pops up windows and redirects the user to our Keycloak SSO to sign in.

These routes are handled in the UI under `routers/auth.ts`

The rest of the UI is bog standard simple fetch requests

## Proxy

The proxy is a Traefik instance that HTTPS/UDP/TCP reverse proxies domain names and ports associated with an instance.

It automatically configures itself by using the Traefik HTTP config provider by requesting one of the API's endpoints that returns the config. (This is secured with a key)

# Development
