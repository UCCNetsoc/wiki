---
title: Hosting Guide - WordPress
description: 
published: true
date: 2021-01-17T20:03:24.285Z
tags: 
editor: markdown
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
	* i.e if you named your instance `beans` it would be: `beans.<username>.container.netsoc.cloud`
* You will be brought to a page to configured WordPress
	* Set your site title
  * Set a username and strong password
  	* **Keep this password safe, resetting it is difficult**
    
# Use your free Netsoc Cloud domain

* Visit the instances list
* Add a vhost via **+ VHost**
* For the host section, enter in:
	* You can use your username domain like: `<username>.netsoc.cloud`
  	* i.e `ocanty.netsoc.cloud`
  * You can use a domain above your username domain like:
  	* i.e `blog.ocanty.netsoc.cloud`
    
* Use Port 80 with internal HTTPS turned off
* Hit **Confirm**

* Visit the domain to ensure everything is working correctly

# Using a custom domain

* Visit the instances list
* Add a vhost via **+ VHost**
* Follow the instructions on what records you need to set

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

# FAQs

## I forgot my password

* Reset the root password via the **Reset Root** button in the instances list
* Hit **Terminal** and enter the username `root` and the password you received via email
* Once the terminal has opened, use WP-CLI by entering the following command to set the password of your WordPress user
	* `wp user update ocanty --user_pass=MySuperSecretPassword`
  * https://developer.wordpress.org/cli/commands/user/update/
  
