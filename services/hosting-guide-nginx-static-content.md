---
title: Hosting Guide - Static Content (with Nginx)
description: 
published: true
date: 2021-01-27T21:57:28.877Z
tags: 
editor: markdown
---


# Initial Steps

* If you have not yet done so, follow the steps to sign up for Netsoc Cloud in the [Tutorial](/services/tutorial)

# Instance Request

* Create a **Container** instance request for the **Linux Tools** image
	* Choose the hostname as you wish (e.g. `website`, `mysite`)
  * Explain what you are using this instance for: for hosting your static content/website
  
* Wait for the request to be accepted/denied

# Setting up Nginx (the web server)

## Configure your free Netsoc Cloud domain

* Visit the instances list
* Add a vhost via **+ VHost**
* For the host section, enter in a subdomain you wish to use:
	* You can use your username domain like: `<username>.netsoc.cloud`
  	* i.e `ocanty.netsoc.cloud`
    
* Use Port 80 with internal HTTPS turned off
* Hit **Confirm**

* Visit the domain to ensure everything is working correctly
	* It may take a few minutes for it to become active

## Configure a custom domain (optional)

* Visit the instances list
* Add a vhost via **+ VHost**
* Follow the instructions on what DNS records you need to set

* On your domain registrar (i.e where you bought your domain from) you will need to set records
	* You can find instructions on how to do this by typically googling "how to set dns records" + the name of your registrar
  	* Examples:
    	* [Cloudflare](https://www.cloudflare.com/learning/dns/dns-records/)
      * [Dyn](https://help.dyn.com/setting-up-dns-for-your-new-website/)
      * [Namecheap](https://www.namecheap.com/support/knowledgebase/article.aspx/434/2237/how-do-i-set-up-host-records-for-a-domain/)
      
* Enter your domain into the form
* Use Port 80 with internal HTTPS turned off
* Hit **Confirm**

* Visit the domain to ensure everything is working correctly
	* It may take a few minutes for it to become active

## Accessing Instance

* Start your instance
* Reset the instance root user password via **Reset Root**
* Open the web **Terminal** and sign in using the `root` username and password received in the email
* You should now see something like this (hostname will match the name of your instance):
	* ![tutorial-minecraft.png](/assets/cloud/tutorial-minecraft.png)
  
## Install Nginx

* Run the command:
	* `apt update && apt install nginx`
  * Then configure nginx to start on boot with `systemctl enable nginx`
  * Then ensure it is started `systemctl start nginx`


# Installing your static content

* Visit the instances list
* Click **Filesystem** on your instance and follow the instructions to open up a file browser
* You can now upload files as you wish
* Any HTML/static content files you place in `/var/www/html` will now be served when you visit the domains you configured above


  
# More Info

* [DigitalOcean guide to Nginx setup & configuration](https://www.digitalocean.com/community/tutorials/nginx-essentials-installation-and-configuration-troubleshooting)

 