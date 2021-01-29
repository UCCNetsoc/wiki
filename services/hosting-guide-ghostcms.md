---
title: Hosting Guide - GhostCMS
description: 
published: true
date: 2021-01-29T00:01:06.005Z
tags: 
editor: markdown
---


# Initial Steps

* If you have not yet done so, follow the steps to sign up for Netsoc Cloud in the [Tutorial](/services/tutorial)

# Instance Request

* Create a **Container** instance request for the **Ghost CMS** image
	* Choose the hostname as you wish (e.g. `myblog`, `ghost`, `website`)
	* Explain what you are using this instance for, i.e why are you hosting Ghost
  
* Wait for the request to be accepted/denied

# Use your free Netsoc Cloud domain

* Visit the instances list
* Add a vhost via **+ VHost**
* For the host section, enter in a subdomain you wish to use:
	* You can use your username domain like: `<username>.netsoc.cloud`
  	* i.e `jac.netsoc.cloud`
    
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

# Setting up Ghost
	
* Start your instance
* Click the link of your desired VHost
  * ![tutorial-ghost1.png](/assets/cloud/tutorial-ghost1.png)

* You will be brought to a page to install Ghost
	* ![tutorial-ghost2.png](/assets/cloud/tutorial-ghost2.png)
	* Ensure the desired VHost is entered into the text box - 
  	* The entered VHost must be correctly configured in the Netsoc Cloud control panel for Ghost to install correctly
	* Hit Set Host to begin the installation
* The installation process should begin
	* ![tutorial-ghost3.png](/assets/cloud/tutorial-ghost3.png)
* Once installation has completed a link to the administration page is shown
	* ![tutorial-ghost4.png](/assets/cloud/tutorial-ghost4.png)
* Clicking the link to the admin page should display the following screen
	* ![tutorial-ghost5.png](/assets/cloud/tutorial-ghost5.png)
	* Select Create your account
* ![tutorial-ghost6.png](/assets/cloud/tutorial-ghost6.png)
	* Set your site title
	* Set your name
	* Set your email
	* Set your password
       * If forgotten this can be changed via a password reset email
* ![tutorial-ghost7.png](/assets/cloud/tutorial-ghost7.png)
	* (Optionally) invite additional staff members
* The site is now ready to use
	* ![tutorial-ghost8.png](/assets/cloud/tutorial-ghost8.png)

# Accessing the admin page

* Visit `<domain>/ghost`
	* i.e `jac.netsoc.cloud/ghost`
