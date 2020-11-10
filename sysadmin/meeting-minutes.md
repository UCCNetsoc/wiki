---
title: Meeting Minutes
description: Summaries of recent SysAdmin meetings
published: true
date: 2020-11-10T19:35:38.181Z
tags: 
editor: markdown
---

# 2020/2021

## Meeting 7 (2020-11-10)

#### Arthan

* Have done anything
	* Waiting on Stephen

* Design still needs to be done
  * Eric might help out with the design

#### Canty/Thomas 

* Backend is up and running, just need to fix some bugs
* File manager backend is finished soon

#### Eric

* Ansible now runs through CI
	* Document pls
  
* Loki/Fluentd
	* Loki has a docker driver
    * We need to test what happens if Loki comes down while the containers are set to log to it
  * We can use fluentd to collect the syslog/journal from the hosts and push to Loki
 
 #### James
 
 * LXC templates
 	 * Simplified, each template has it's own provisoning file and not using the same roles for the same distros
   * GhostCMS, heavily push towards to stop people installing awful Wordpress
   * Pushing so we can do our own templates
   
 * Grafana
   * More worked
 
 #### Everyone
 
 * Lockdown is over 1st December
    * 2 weeks from now 24th
    
 * Server hardware 
    * Option 1
       * Buy used Dell Hardware
          * R730s, R630s
         	* Going with SFF SSDs,
    * Option 2
       * Will get a quote
 * Disk spreadsheet
 
 * Need to make a list of stuff to be done at CIX
 
## Meeting 6 (2020-10-23)

#### Canty/Thomas
* Netsoc Cloud progress
	* Backend done
  * UI now
  * File manager integration
  	* Canty pushing changes tonight to the new branch
  * Eric doing UI
  * Beta test soon when everythings done
  	* Invite like 10-15 people
    * Split groups in 50/50 Container/VPS
    * Get a grouping of use cases
    	* Dev
      * Blogger Art Student lmao
      
#### James
* LXC templates
	* Packer really isn't an option
  * James says there's an alternative
  	* Looking into it more
    
* Grafana
  * Grafana 7 & Grafana 6 weirdness
  * Have to mix and match jsonnet for grafana 7 and grafana 6
  * new.grafana.netsoc.dev

#### Eric/Thomas
* Loki
	* Running but untested
  * with fluentd
 
* AWX
	* It's borked
  
#### Everyone
* Hardware news,
  * Need to ask for the purchases
  
	* UCC VMs on hold until November 2th.
  * We want to install FreeIPA + Keycloak on VM1
  * Motley+Express+Sexpress on VM2
  	* Document student media meticulously

#### Arthan
* Alumni Donation Page
	* Arthan
  * Needs to stand the test of time
  * Should let us take donations with Ticketsolve
  * Testimonials from past HLMs/SysAdmins/Chairs/Ex-Officios
  	* Json file stored in the GitHub repo
    	* Face picture
      * Name
      * Position
      * Testimonial
      * Years active
    * Testimonials submited by PR/Issue
  * Historical photos
   
#### Reece <3

* Adding a feature to scrape events from facebook to automatically add them to the discord bot
 

## Meeting 5 (2020-10-16)

* Present
	* Thomas
  * Oisin C
  * Eric 
  * James
  
  
* Netsoc Admin
	* We're moving to doing 'cloud', we want to focus on more becoming an infra-provider than service provider
  * i.e think AWS Lightsail
    	![](https://cdn.discordapp.com/attachments/710613627081850920/766460331299307520/ls_option_linux_app_1.png =480x)
  * We're gonna offer LXC containers and VPS hosted on Proxmox with port mapping to a single external IP
  * Users are still registered in portal and will ssh into their vm from there 
  * We can offer users various templates to install stuff
  * For LXC, we want to generate these templates (hopefully) using Packer and Ansible and store the resulting tar.gz in a public GitHub repo
  * We'll force-push the repo to 1 commit to keep the repo size down
  * Templates we need to make:
     * WordPress (alpine)
     * Ghost CMS 
     * LEMP - linux nginx mysql php
     * Node.js
     * Nginx static
     * Python CGI (lmao)
     * Minecraft
     * CSGO
     * Garry's Mod
     * TF2
     * `/var/lib/vz/template/cache` test em here
  * VM templates
     * Just base ubuntu for now
     * Ubuntu
     * CentOS
     * Arch
     * Debian
* Netsoc Admin File Manager

* CIX Trip Tasks (in this order)
  * tesla.netsoc.co is being reimaged
     * We need to move student media over to it
     * Student media database is on bigbertha
     * Motley/express/sexpress/etc are on /media/somedrive on leela
     * We need to document this setup heavily
  * We can then put Proxmox on leela and not worry about killing student media
  * also gives an opportunity to decomission bigbertha because smedia database is now on tesla
  * Server Hardware
     * We we want to buy 2 servers
     * Preferably minimum 4x HDD 3.5inch bays
     * 48-64gbs RAM DDR4.
     * Preferably Dell/HP
     * Also need to buy 16gb ram for lovelace
     ![](https://cdn.discordapp.com/attachments/569930801416765480/764417494806822922/Screenshot_20201010-102042.jpg)
     * Want to sort out hard drive situation on servers
  
* Cloudflare DNS diffing now works (yay!)

* Tasks for this week:
* Oisin C.
	* Getting port mapping and traefik working with cloud
* Eric
	* LXC images
  * AWX
  * Frontend for VMs/file manager
* Thomas
  * Backend for the file manager
  * AWX with eric
* James
  * Proxmox VM dashboard on grafana
  * Grafana/grafonet
  * LXC images with eric
  
* EVERYONE
  * Find server deals, ye have 2 weeks from Monday
  * Favor r630s, r730s. r730xd MUST have 3.5inch HDD bays

* Other:
  * meetings now held weekly

## Meeting 4 (2020-10-02)

* netsocadmin ready-ish
	* just need to setup traefik routes
  * canty is porting users
  * splitting into duo docker containers
 
* pterodactyl games
	* we forked pterodactyl
  * keycloak federation with socialiate
  
* backups are setup
	* playbook that sshs in from a remote VPS
  * uploads gdrive key of the head sysadmin
  * weekly backups of every VPS
	* documented on the wiki

* netsocadmin 
	* file manager feature
  * see issues
  * thomas/arthan backend
  * eric frontend
  
* k8s setup
	* next weekend, eric / james / canty
 

## Meeting 3 (2020-07-27)

* Canty fixed everything broken
	* See [blameless-postmortems](/sysadmin/blameless-postmortems) for 2020-07-20
  
* Eric
	* Backups working-ish 
  	* Cronjob from external server to ru n
  * Moved over minecraft
  * working on a possible solution code-server
  * Pterodactyl, daemon
  
* James
	* Prometheus working
  	* Exporters working
    * Can assign them by ansible groups
    * Prometheus server:
    	* Won't use NFS at all, it complains about file handles
      * Putting the server on `databases` vm
    * Local setup for grafana and grafonet working, will port over
    

* Thomas
	* Moving CI, consul and vault
  * Helping eric with code-server
  * Setting up auditd on `portal`:
  	* https://github.com/Neo23x0/auditd/blob/master/audit.rules
  
* Arthan
	* https://github.com/UCCNetsoc/discord-bot/pull/23
  * Prom PR
	* Adding prometheus to dev-env
	* Minecraft Hunger Games
  	* Custom plugin

* Oisin A
	* Minecraft Hunger Games
  	* Custom Bukkit plugin
    
* Oisin C
	* Sorting out netsocadmin stuff
  	* Had to ditch golang because seteuid support
  * Adding freeipa and keycloak to dev-env
 
   

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