---
title: Tutorial
description: A guide on how to get started using the UCC Netsoc services
published: true
date: 2021-01-16T01:53:33.704Z
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
  * A static website
    * Nginx
  * A CMS based blog/website:
    * GhostCMS
  * A game server:
    * Minecraft
    * CS:GO
  * A Linux development box (i.e to use Docker and other programming languages)
  
# Requesting Instances

* An instance is a running installation of a Linux server
* We offer instance images to save you the time of setting everything up

* An instance can be a **Container** or a **Virtual Private Server**
	* The vast majority of users will only need a **Container**
  
## Requesting an Instance

* Enter the Instances panel
* Select the **Request** button in the **Containers** list

# Managing Instances

![tutorial-instance.png](/assets/cloud/tutorial-instance.png)

## Start/Stop/Shutdown an instance

* You can only access and run most actions while an instance is running

## Accessing your instance (via the `root` user)

* You will to hit **Reset Root** to get a root password for the instance!
* Hit **Confirm** to have the root password reset for this instance sent to your student email
![tutorial-root.png](/assets/cloud/tutorial-root.png)

### Access via the web terminal

* Hit **Terminal** and follow the instructions
* Enter username `root`, and the password in the root user email you received when prompted
	* The terminal will appear like below:
	* ![tutorial-webterm.png](/assets/cloud/tutorial-webterm.png) 

### Access via SSH

* Add a port mapping using **+ Port** and map an external port to **Port 22**
	* ![tutorial-portmap22.png](/assets/cloud/tutorial-portmap22.png)
  
* You will see the port map is now in effect
	* ![tutorial-portmap-result.png](/assets/cloud/tutorial-portmap-result.png)
* Use a terminal `ssh` command:
	* Open a command prompt:
  	* On Windows: search for `cmd` in the Start Menu and open `Command Prompt`
    * On Linux: open a terminal
  * Enter the following command:
  	* `ssh root@cantybox.ocanty.container.netsoc.cloud -p16537`
  * Hit yes to any message about trusting keys
  * Enter the root password you received in the email
  	* You may want to change the root password once logged in via the `passwd` command
    

### Accessing the file system

* Hit **Filesystem** and follow the instructions

## Instance Expiry

* You need to renew your instance so it does not expire

* The expiry date and renewal button can be seen as below:
	* ![tutorial-activation.png](/assets/cloud/tutorial-activation.png)

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
  