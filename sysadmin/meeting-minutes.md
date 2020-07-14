---
title: Meeting Minutes
description: Summaries of recent SysAdmin meetings
published: true
date: 2020-07-14T03:05:55.630Z
tags: 
editor: undefined
---

# 2020/2021

## Meeting 2 (2020-07-13)

* Eric status on backup jobs
  * Currently backs up to GDrive and vzdump can ignore files
  * Changing to ignore disks rather than files
  * See: https://github.com/UCCNetsoc/NaC/pull/16
  	* https://github.com/UCCNetsoc/NaC/issues/8

  * Moving Minecraft over from legacy to proxmox setup
  	* https://github.com/UCCNetsoc/NaC/issues/13

* Arthan
  * Haven't had time do much
  * Prom expoter is counting members as they come and go
  * Number of members
  * Only been testing by curling /metrics
  * Adding Prometheus to dev-env to test correctly

* James
  * All exporters working,
  * Needs to setup Protetheus
  * Setting up Grafana + jssonet/grafonnet
  	* https://github.com/UCCNetsoc/NaC/issues/11
  * Merged !online support to the bot, F to DiscordSRV

* Thomas
  * Wiki done
  * Moving CI to docker_swarm_services

* Canty
  * Setting up Pterodactyl + automating keycloak export
  	* https://github.com/UCCNetsoc/NaC/issues/13

* Future idea discussion:
  * Hosted Vscode
    * Could let users edit their code online
    * might have issues with security
    * Eric investigating

  * More resources for running stuff  
    * Current work being done on Wiki

* netsocadmin custom domains
	* Canty found a way to make bind return the same IP for every lookup
  	* Could be an alternative to writing a custom nameserver in Go

---

## Meeting 1 (2020-07-06)

* Document _everything_ for PR purposes
  * We have the potential to have one of the best society server setups in Ireland
  * Document this and make it public. Wiki, blog posts, twitter, etc...

* Tasks:
  * Eric
    * We need to setup a backup job with Duplicaty
      that backs up the backup jobs performed by Proxmox 
      * Needs to compress all the backups and upload to GDrive
        * Tarring + Compression is important because many of the backups are similar as they are 
          all clones of the same Ubuntu template

  * Arthan
    * Prometheus Exporter for the Discord Bot
      * No. members
      * No. events
      * No. messages
      * Most active users (count)
      * Messages per channel
* Thomas+Canty
    * Wiki wikijs

  * James
    * Prometheus setup
      * Need to make a test vm called
          testvm.vm.netsoc.co

      * Make a playbook to setup these containers
        * node_exporter 
        * cadvisor          
        * expexp_exporter   8000

      * On the docker swarm playbook 
        * Prometheus server

        * Configure Prometheus to read the expexp of testvm.vm.netsoc.co

* TBD next meeting
  * Move existing services over:
    * website
    * blog
    * ci
    * etc
    * games-minecraft

  * Grafana
    * jsonnet to read from Prometheus?
  * Pterodactyl
    * Waiting on a PR to merge OAuth/OIDC/Keycloak support

* Future possible ideas:
  * netsoc.dev sysadmin portal
      simple sysadmin portal
      quick links for sysadmins
  * hosted vscode
  * openfaas? 
  * more resources/documentation for running stuff like
    * for nfinx unit custom runtimes
    * flask
  	* this should be on all the wiki
  * "we shouldn't provide a service if no-one knows how to use it"
  * blog post on how to get started in nsa

* Other discussion:
  * Custom domain implemention for netsocadmin websites API:
    * Option 1 - simple, but we can't change IPs without the user doing it themselves
      go into your domain panel and set these:

      A 81.45.X.53 - leela
      A 81.45.X.52 - lovelace 

    * Option 2 - set their domain nameserver to our custom DNS server implementation
      websites.netsoc.co NS 
      We can return any records we want but we need to given them the option to modify
      their own records or they won't be able to host their own email / subdomains / etc