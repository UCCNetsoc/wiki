---
title: Meeting Minutes
description: Summaries of recent SysAdmin meetings
published: true
date: 2021-03-11T20:09:44.017Z
tags: 
editor: markdown
---

# 2020/2021

## Meeting 14 (2021-03-11)

* WebSSH2 doesn't work on firefox
	* if a user signs in, the credentials get saved even if they're incorrect
  * firefox won't clear them, (it might 403 forbidden)
  * Alan
  
* get link shortener working
  * Thomas

## Meeting 13

* I forgot what we did

## Meeting 12 (2021-01-24)

* HLM nomination form
	* basic gist:
  	 * name of the person who's nominating the person
     * name of the person being nominated 
     * reason
     * email it to netsoc - uccsocieties.ie
  * Arthan
  * March
  
* CIX visit:
	* Leela is now a proxmox host
  * Still need to sort out storage + firewall
* Next CIX visit:
	* 64gb of ram to go into leela, can invetigate the backplane issues
  * Date: covid dependant (aka never)

* James:
	* GhostCMS template
  * Prometheus exporter for container

* Eric:
	* Dev-env, proxmox simulation
  * MySQL dead

* Alan & Thomas
	* Add to CI
  * Set up DNS 
  * Set it up in provision-infra-web.yml


## Meeting 11 (2020-12-10)

* Stop using FreeIPA DNS

#### CIX visit

* Need to sort out CIX visit
* Maybe on the December 21st or 23rd

* Proxmox on leela
* Sorting out HDDs

#### Arthan

* Grafana dashboards

#### Canty

* Removing FreeIPA DNS
	* https://github.com/UCCNetsoc/NaC/blob/a343ec9c22e0e5cf541a85cb041f47f08514979e/roles/grafana/files/provisioning/datasources/prometheus.yaml#L14
  * https://github.com/UCCNetsoc/NaC/blob/946b2ad6eb5cfc4e53ce527252374cb8d6c0064b/provision-infra-web.yml#L258
* MySQL template nearly done
	* Go bootstrap thing

#### Eric

* Grafana works with loki
* Vagrant Proxmox working
	* DNS for the Proxy -> Proxmox

#### James

* NodeJS template done
* /lxc random folder is done
* Grafana in progress

#### Thomas + Alan

* Writing Docker container for MonstaFTP 
* Modifying it so we can autofill the form by URL

* URL shortener
	* Supports multiple URLs 
  
* Traefik setup

#### Dashboard

* James
* Eric
* Arthan
* Alan

* Monitoring Proxmox hosts
* Monitoring Infra VMs
* Monitoring User VMs

## Meeting 10 (2020-12-02)

#### Arthan

* Not here

#### Eric

* Vagrant Proxmox setup
	* Networking works
* Cloud File manager
	* API done (ish)
  * Canty will push a skeleton of the file manager UI, eric will finish it
* git to mean

#### Oisin

* Wordpress&LEMP templates done
* Templates done:
  * Developer Tools
  	* has categories: medium
  * GhostCMS (broken)
  	* 512
  * Wordpress
  	* 512
  * LEMP
  	* has categories: small, medium
  * LinuxGSM 
  	* has categories: medium, large, huge
  * MEAN
  	* has categories: small, medium
  * Nginx
  	* has categories: small
    
  * OS
  	* Ubuntu/Debian/CentOS/Alpine/Arch/OpenSUSE
    * small, medium, large
    
* To be done:
	* Nodejs
  * MySQL
  

* Small:
	512mb 10gb 1cpu
* Medium
	1gb 15gb 1cpu
* Large
	2gb 20gb 2cpu
* Huge
	4gb 25gb 4cpu
 

* Need to create skeleton file manager UI

#### James

* Templates have /lxc copied to the host with a random folder name now so multiple people can build them simulatenously
	
* Grafana:
	* Ditch graffonnet
  * Provision all dashboards via NaC
  * When a change is made copy it back into NaC and run grafana playbook
  
#### Thomas + Alan

* URL shortner
	* POST https://links.netsoc.co/
    * `{ domain: "https://netsoc.events/", slug: "whatever", target: "https://wiki.netsoc.co/thing" }`
    * `https://netsoc.events/whatever`
    
#### Alan

* "orientation"
	* Friday 7pm

## Meeting 9 (2020-11-25)

#### Arthan

* Minecraft server setup, someone needs to set spawn protections
* Running 1.16.4

#### Oisin

* Netsoc Cloud
  * We need to send out beta sign up Google Form
  * File manager, canty pushing changes tonight or tomorrow night
  * Template status
  	* Templates w/ allocations:
      * WordPress - still needs to be done
          * 1CPU, 512MB RAM/512 SWAP
      * GhostCMS - james
      	* 1CPU, 512MB RAM/512 SWAP
      * NodeJS - james
      	* 1CPU, 512MB RAM/512 SWAP
      * LEMP stack - canty
      	* 1CPU 512MB/512
      * MEAN stack - eric
      	* 512MB
      * MySQL - canty
      	* 512MB
      * Devtools - eric
      	* git, gcc, gnupg, clang, python3, gdb, ruby, node, npm, golang, rust, vim, emacs, jq, gdb, curl, sqlite
        * 1024MB
      * LinuxGSM - eric
      	* 2CPU, 2048MB / 2048MB Swap
      * Ubuntu/Debian/CentOS/Alpine/Arch/OpenSUSE - already done with proxmox
      	* 1024MB
    

* CIX trip
  * Want to prioritize Thomas + First Year Sys
  * Canty making an _ordered_ list of what needs to be done
  * Need to a pick a date when everyone is available
     * Maybe end of semester 1? 17th December
     * or earlier in December
  
#### Eric

* Templates done
* File manager

#### Jotter

* Moved over Grafana
* /lxc stuff

#### Thomas

* Link shortener
	* Needs to support 'branded' urls like netsoc.events/phreaking
  * Needs to support multiple domains
  * API agaiinst PG domain
* Integrate into discord bot for committee

## Meeting 8 (2020-11-18)

#### Oisin/Thomas

* Netsoc Cloud
	* It's deployed! Containers work, everything works bar VPS password resets
  * Planning on doing a beta within the next few weeks
  	* Arranging a google form for the announcements section
    * What sort of people should we get for the beta?
    	* Techy people first
      * Maybe invite many groups of many different people later
    * Templates we should have ready for the beta
    	* WordPress - canty
      * GhostCMS - james
      * NodeJS - james
      * LEMP stack - canty
      * MEAN stack - eric
      * MySQL - canty
      * Devtools - eric
      	* git, gcc, gnupg, clang, python3, gdb, ruby, node, npm, golang, rust, vim, emacs, jq, gdb, curl, sqlite
      * LinuxGSM - eric
      * Ubuntu/Debian/CentOS/Alpine/Arch/OpenSUSE - already done with proxmox
    * File manager
    	* Canty needs to push code changes so Thomas can start integrating his changes (sorry!)
 
 #### Eric
 
 * Loki is logging everything for Docker
   * Promtail used instead of Docker driver
      * pls add `/netsoc/freeipa/var/log/*.log`
   * Promtail syslogs on hosts
 
 #### Thomas
 
 * File manager downloading directories
 
 #### James
 
 * Gonna setup grafana via webui
   
 #### Everyone
 
 * Server purchase decisions, next week
 
#### Reece :heart:

* Doing design for the frontend of the alumni page

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