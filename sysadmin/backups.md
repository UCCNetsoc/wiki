---
title: Backups
description: 
published: true
date: 2021-04-15T15:48:20.532Z
tags: 
editor: undefined
---

# VM Offsite Backups

> Ensure you are familiar with the [Netsoc As Code](/sysadmin/netsoc-as-code) and the Netsoc Infrastructure before deploying the offsite backup script.
Since Netsoc uses Proxmox to manage all our VMs, we use `vzdump` to create compressed archives of individual VMs. We then sync the backups to a UCC Google Drive account using `duplicity`.
We have the backup scripts merged into the [Netsoc as Code repository](https://github.com/UCCNetsoc/NaC). 
To run it, ensure you have the following requirements:

1. A UCC Gsuite account
1. A secure remote server or VPS that you trust with having root ssh keys and access tokens on

## Changing Google Drive Account
1. On your local machine, install `duplicity` and `pydrive`.
1. Create the files `.duplicity/credentials` and `.duplicity/excludes`
1. From the secrets in NaC, get the `gdrive.client_id` and `gdrive.client_secret`.
1. Put the following contents into `.duplicity/credentials`, replacing the id and secret with the entries in NaC: 

        client_config_backend: settings
        client_config:
           client_id: <client_id>
           client_secret: <client_secret>
        save_credentials: True
        save_credentials_backend: file
        save_credentials_file: ~/.duplicity/gdrive.cache

1. Run `GOOGLE_DRIVE_SETTINGS=~/.duplicity/credentials duplicity --exclude-filelist ~/.duplicity/excludes ~/.duplicity gdocs://<your-student-number>@umail.ucc.ie/duplicity_setup --no-encryption`
1. Copy the contents of `~/.duplicity/gdrive.cache` into `gdrive.cache` in NaC secrets.


## Installation

1. On your remote server, `su root` and ensure you are in `/root`.
1. ssh into the control server to ensure it's added to `known_hosts`, then exit it.
1. Ensure crond is running and installed: `systemctl status crond` or `service crond status`.
1. Clone the NaC repository: `git clone https://github.com/UCCNetsoc/NaC.git`.
1. In `~/NaC`, create the directory `keys/<name-of-control-host>` and put a valid root ssh key for the control host into it. 
1. Run `ansible-playbook --ask-vault-pass setup-backups.yml`

If all goes well, backups should now start running on a designated schedule :)
