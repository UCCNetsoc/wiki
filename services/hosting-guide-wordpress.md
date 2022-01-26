---
title: Hosting Guide - WordPress
description: 
published: true
date: 2021-06-04T00:06:09.290Z
tags: 
editor: markdown
dateCreated: 2021-01-17T20:03:24.285Z
---

# Initial Steps

* If you have not yet done so, follow the steps to sign up for Netsoc Cloud in the [Tutorial](/services/tutorial)

# Instance Request

* Create a **Container** instance request for the **WordPress** image
	* Choose the hostname as you wish (e.g. `myblog`, `wordpress`, `website`)
  * Explain what you are using this instance for, i.e why are you hosting WordPress
  
* Wait for the request to be accepted/denied

# Setting up WordPress
	
* Start your instance
* Click the link on the default VHost
  * ![tutorial-wordpress1.png](/assets/cloud/tutorial-wordpress1.png)

* You will be brought to a page to configure WordPress
	* ![tutorial-wordpress2.png](/assets/cloud/tutorial-wordpress2.png)
	* Set your site title
  * Set a username and strong password
  	* **Keep this password safe, resetting it is difficult (email based password reset is disabled)**
  * Hit Install WordPress
    
# Use your free Netsoc Cloud domain

* Visit the instances list
* Add a vhost via **+ VHost**
* For the host section, enter in a subdomain you wish to use:
	* You can use your username domain like: `<username>.netsoc.cloud`
  	* i.e `ocanty.netsoc.cloud`
    
* Use Port 80 with internal HTTPS turned off
* Hit **Confirm**

* Visit the domain to ensure everything is working correctly
	* It may take a few minutes for it to become active

# Using a custom domain (optional)

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

# Accessing the admin page

* Visit `<domain>/wp-admin`
	* i.e `ocanty.netsoc.cloud/wp-admin`

# FAQs

## I forgot my password

* Reset the root password via the **Reset Root** button in the instances list
* Hit **Terminal** and enter the username `root` and the password you received via email
* Once the terminal has opened, use WP-CLI by entering the following command to set the password of your WordPress user
	* `wp user update <username> --user_pass=MySuperSecretPassword --allow-root --path=/var/www/html`
  		* e.g. `wp user update ocanty --user_pass=MySuperSecretPassword --allow-root --path=/var/www/html`
  * https://developer.wordpress.org/cli/commands/user/update/
  
