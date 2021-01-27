---
title: Tutorial
description: A guide on how to get started using the UCC Netsoc services
published: true
date: 2021-01-27T22:00:42.633Z
tags: 
editor: markdown
---

# Netsoc Cloud?

![tutorial-logo.png](/assets/cloud/tutorial-logo.png)
* Netsoc Cloud is your access to UCC Netsoc's hosting services
* Provision containers and VPSes

* You can do things like
 	* Run WordPress or Ghost CMS blogs and websites
	* Run a development server and develop web applications
	* Run a game server like Minecraft, CS:GO, FiveM
  * Run Linux applications as you wish
  
* This is similar to the services offered by VPS & Cloud hosting services like DigitalOcean, AWS, & RamNode
* The aforementioned services cost a monthly fee, we offer them for free to any member of UCC Netsoc

# Signing Up & Logging In

* Visit the Netsoc Cloud management panel by going to [netsoc.cloud](https://netsoc.cloud)
* Hit **Sign Up** to show the sign up form
  * ![tutorial-signup.png](/assets/cloud/tutorial-signup.png)
  * Enter your student email address (i.e studentid@umail.ucc.ie)
  * Enter a username
  	* This will be **publicly available and seen by others** and **cannot be changed later**, so choose wisely
  * You need to be a member of UCC Netsoc to use our services
  	* Visit the [Clubs & Socs Portal](https://candsportal.ucc.ie/) to become a member
  * Accept the [Terms of Service](/services/terms-of-service)
  * Complete the "I'm not a robot" verification
* Hit **Sign Up** to create your account
* You will recieve an email (sent to your student email) with an activation link 
  * ![tutorial-email.png](/assets/cloud/tutorial-email.png)

* Click the activation link and you will brought to an activation form
	*  ![tutorial4.png](/assets/cloud/tutorial4.png)
	* Enter a strong password for your account
  * Hit Confirm
  * If your activation code has expired, you can hit **Resend**, to resend the activation link
  

# Logging into Netsoc Cloud

* Visit the Netsoc Cloud management panel by going to [netsoc.cloud](https://netsoc.cloud)
* Hit **Login** to open the login form
	* ![tutorial5.png](/assets/cloud/tutorial5.png)
  * Enter your username & password and hit **Log In**
  
# Hosting Guides

* Select one of the guides below to set up what you need, as you need:
  * A CMS based blog/website:
  	* [WordPress](/services/hosting-guide-wordpress)
    * GhostCMS
  * A game server:
    * [Minecraft](/services/hosting-guide-minecraft)
  * A static website
    * [Nginx](/services/hosting-guide-nginx-static-content)
  * A Linux development box (i.e to use Docker and other programming languages)
    * [Development Box](/services/hosting-guide-development-box)
  
# Setting up services: Instances

## **An instance is a running installation of a Linux server**
* We offer instance images to save you the time of setting everything up

* An instance can be a **Container** or a **Virtual Private Server**
	* The vast majority of users will only need a **Container**
  
## Requesting an Instance

* Enter the Instances panel
* Select the **Request** button in the **Containers** list
	* ![tutorial-instance-request.png](/assets/cloud/tutorial-instance-request.png)

* Select an instance you would like
	* Enter a hostname for the instance
  	* Must be alphanumeric with no spaces
  * Enter why you would like the instance
    * Take note of some of the stricter requirements:
    	* ![tutorial-tos.png](/assets/cloud/tutorial-tos.png)
    * Consult the Terms of Service for more information
  * Hit **Confirm**
  
* We will receive the instance request and we will accept or deny it
	* You will receive an email when this happens
 
* Once your instance is accepted, it will appear in the list of your instances and you will be free to control it

# Managing Instances

![tutorial-managingbetter.png](/assets/cloud/tutorial-managingbetter.png)

## Start/Stop/Shutdown an instance

* You can only access and run most actions while an instance is running

## Resetting the instance `root` user

* You will need to hit **Reset Root** to get a root password for the instance!
* Hit **Confirm** to have the root password reset for this instance sent to your student email
![tutorial-root.png](/assets/cloud/tutorial-root.png)

## Access your instance via the web terminal

* **You will need to reset the root user before doing this, follow the instructions above!**
* Hit **Terminal** and follow the instructions
* Enter username `root`, and the password in the root user email you received when prompted
	* The terminal will appear like below:
	* ![tutorial-webterm.png](/assets/cloud/tutorial-webterm.png) 
  * You may want to change the root password once logged in via the `passwd` command

## Access your instance via SSH (advanced)

* **You will need to reset the root user before doing this, follow the instructions above!**
* Add a port mapping using **+ Port** and map an external port to **Port 22**
* You will see the port map is now in effect
	* ![tutorial-portmap-result.png](/assets/cloud/tutorial-portmap-result.png)
* Use a terminal `ssh` command:
	* Open a command prompt:
  		*	On Windows: search for `cmd` in the Start Menu and open `Command Prompt`
    	* On Linux: open a terminal
  * Enter the following command using the external port you mapped (like the screenshot above)
  	* `ssh root@cantybox.ocanty.container.netsoc.cloud -p<external port>`
      * e.g. `ssh root@cantybox.ocanty.container.netsoc.cloud -p16537`
      * This will connect to your instance on the port you exposed earlier
    * ![tutorial-ssh-windows.png](/assets/cloud/tutorial-ssh-windows.png)
  * Hit yes to any message about trusting keys
  * Enter the root password you received in the email
  	* You may want to change the root password once logged in via the `passwd` command

## Accessing your instance file system

* **You will need to reset the root user before doing this, follow the instructions above!**
* Hit **Filesystem** and follow the instructions

## Adding a port forward/mapping

* Select **+ Port** and follow the instructions
 	* ![tutorial-portmap22.png](/assets/cloud/tutorial-portmap22.png)
  
## Adding a domain

* You can forward any web (HTTP/HTTPS) traffic from the web into your instance

* Select **+ VHost** and follow the instructions
 	* ![tutorial-vhost-new.png](/assets/cloud/tutorial-vhost-new.png)
* You should input the domain you want in accordance to the instructions
* The internal port should be the port that the web server running on the instance is listening on.
	* i.e typically 80, 8080

## Upgrading your instance

* Hit the **Upgrade** button and let us know what new specs you require
	* Upgrade approval subject to the extent of your request and the resources available to us and other users

## Instance Expiry

* You need to renew your instance so it does not expire

* The expiry date and reactivation button can be seen as below:
	* ![tutorial-activation-new.png](/assets/cloud/tutorial-activation-new.png)

* If your instance expires it can be forcefully **shutdown** or **deleted!**
	* Make sure to keep personal backups of your data

# FAQs

### I haven't received my activation link

* It may take 10-15 minutes for your activation email to arrive
* Check your junk/spam

* If you still have not received it:
  * Visit the Netsoc Cloud management panel by going to [netsoc.cloud](https://netsoc.cloud)
  * Hit **Login** to open the login form
  * Select **Resend Activation** and complete the form that opens
  
### I've forgotten my password

* To reset your password:
	* Visit the Netsoc Cloud management panel by going to [netsoc.cloud](https://netsoc.cloud)
	* Hit **Login** to open the login form
  * Hit **Reset Password** to open a window to reset your password
  