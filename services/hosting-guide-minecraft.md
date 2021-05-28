---
title: Hosting Guide - Minecraft
description: 
published: true
date: 2021-05-28T21:46:21.689Z
tags: 
editor: markdown
---

# Initial Steps

* If you have not yet done so, follow the steps to sign up for Netsoc Cloud in the [Tutorial](/services/tutorial)

# Instance Request

* Create a **Container** instance request for the **Docker** image
	* Choose the hostname as you wish (e.g. `minecraft`, `myminecraft`, `creeper`)
  * Explain what you are using this instance for: a Minecraft server
  
* Wait for the request to be accepted/denied

* Request an upgrade to your specs via the **Upgrade** button ask for an adaquete amount of RAM:
	* e.g. 1gb if you're looking for a 8-12 player server

# Accessing Instance

* Start your instance
* Reset the instance root user password via **Reset Root**
* Open the web **Terminal** and sign in using the `root` username and password received in the email
* You should now see something like this:
	* ![tutorial-minecraft.png](/assets/cloud/tutorial-minecraft.png)

# Setting up the Minecraft Server

## Set up a port map

* We plan on binding Minecraft to port `25565` on our instance
* But we need it to be publicly available so we can join, therefore we need to do a port forward
* Hit **+ Port** and add a port map, which will map a free external port to your internal `25565`
	* ![tutorial-minecraft3.png](/assets/cloud/tutorial-minecraft3.png)

* You will need to remember the external port listed here later

## Run Minecraft (inside Docker)

* Run the command:
	* `docker run -d -p 25565:25565 -e ENABLE_AUTOPAUSE=TRUE -e EULA=TRUE --restart always --name mc itzg/minecraft-server:java16-openj9`
	* This will download a Docker image (think of it like an easy way to package software) 
  * It will then run Minecraft and bind it to the port given
  
  
### Accessing the console

* Run `docker exec -i mc rcon-cli` and then type `help` to see commands
* Press Ctrl-C to exit the console

### Watch the server logs

* Run `docker logs mc --follow`
* Press Ctrl+C to stop following logs

## Connecting to the server

* You can connect to the server by using your instance hostname and externally mapped port
	* ![tutorial-minecraft4.png](/assets/cloud/tutorial-minecraft4.png)

* For the screenshot above, the server and port to join would be `minecraft.ocanty.container.netsoc.cloud:17226`
	* ![tutorial-minecraft6.png](/assets/cloud/tutorial-minecraft6.png =300x)

* If your server appears down, you may need to connect to it once for it to appear online
	* ![tutorial-minecraft5.png](/assets/cloud/tutorial-minecraft5.png)

## Important (autopause) functionality

* You configured this container to pause the server when no-one is on it
* This might occasionally cause you to have to connect to the server (that appears offline) at least once to boot it
* Once rebooted, the server should come up and you should be able to join
* Do **NOT** disable this functionality or we will consider it a ToS violation	
	* This is done to ensure that game servers do not take away resources from other users

## More info & advanced usage

* https://github.com/itzg/docker-minecraft-server/blob/master/README.md
	* Substitute `latest` with `java16-openj9` in commands given here, `java16-openj9` will be more efficient than the default
		* An exception is made for modded Forge servers which are not compatible with `openj9` images.
  * If using `docker run` with these images, you **MUST** have `-e ENABLE_AUTOPAUSE=TRUE`
 
# FAQs

## I can't access this while connected to eduroam

* Eduroam blocks these port ranges, you will need to find a VPN to connect
