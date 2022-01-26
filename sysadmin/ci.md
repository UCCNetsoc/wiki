---
title: Continuous Integration
description: Netsoc's CI / CD
published: true
date: 2021-06-04T00:06:26.464Z
tags: sysadmin, ci, cd, netsoc
editor: markdown
dateCreated: 2021-03-31T20:17:10.574Z
---

# Introduction to CI
**Continuous Integration** (CI) is the process of pushing code into centralised repositories on a regular basis where we can run tasks to handle the new code. 

Tasks can include:
- Building the code 
- Testing the code (See [unit tests](https://en.wikipedia.org/wiki/Unit_testing) (We don't do this usually because we're lazy :p))
- Packaging the code
- Deploying packaged release to production (Continuous Deployment)

# How Netsoc Uses CI

- Netsoc hosts [Drone](https://www.drone.io/) as a CI solution. Drone hooks into [GitHub](https://github.com/uccnetsoc) to listen for events such as code pushes. 

- To access our drone instance, go to https://ci.netsoc.co and sign in with your GitHub account.

- Drone must be enabled on a repository level to start running tasks. Since we deploy Drone via [Netsoc as Code](/sysadmin/netsoc-as-code) (NaC) we must add the repository into the deployment playbook for it to persist:

  [provision-infra-web.yml](https://github.com/UCCNetsoc/NaC/blob/a570cabc13247d846632a6794917e173157dd583/provision-infra-web.yml#L879)
  ```yml
      - name: CI - Create repos dict
        set_fact:
          ci_repos:
            - discord-bot
            - dev-env
            - netsoc.co
            - keycloak
            - drone-ansible
            - cloud
            - webssh2
  ```
  
- Drone reads a `.drone.yml` file in the root directory of the repository as a configuration file to run tasks from.

- Drone contains numerous organization-wide secrets (variables hidden for security reasons) such as keys to Docker Hub, SSH keys to the production VMs, etc.

- Usually Drone is used to build the [Docker](https://docs.docker.com/get-started/overview/) Images for both production and development. 
  Production images (tagged `latest`) usually contain minimal bloat (useless files) and just what's necessary for running it while development images (tagged `dev-env`) are used by the [dev-env](https://github.com/UCCNetsoc/dev-env) to allow for developers to hot reload the code in a running container and recompile it.
  
- Netsoc deploys exclusively via [NaC](/sysadmin/netsoc-as-code) which runs on [Ansible](/sysadmin/intro-to-ansible). As such if we want to have Continuous (automatic) Deployment (CD) after code is pushed to master, Drone must run the Ansible playbook for for that code.
	The following is sample code that must be added to a repo's `.drone.yml` for CD in Netsoc:
  ```yml
    - name: clone_nac
    image: docker:git
    commands:
      - git clone https://github.com/UCCNetsoc/NaC.git .ansible
      - mkdir -p ./keys/infra/web/  # Repeat this mkdir, printf and chmod combination for each host required for deployment
      - mkdir -p ./keys/infra/databases/
      - printf '%s\n' "$KEY_WEB" >./keys/infra/web/id_rsa
      - printf '%s\n' "$KEY_DATABASES" >./keys/infra/databases/id_rsa
      - chmod 0600 ./keys/infra/web/id_rsa
      - chmod 0600 ./keys/infra/databases/id_rsa
      - ls -al ./keys/infra/web
      - ls -al ./keys/infra/databases
    environment:
      KEY_WEB:
        from_secret: key_web
      KEY_DATABASES:
        from_secret: key_databases
    when:
      event:
        - push
      branch:
        - master

  - name: ansible_deploy
    image: uccnetsoc/drone-ansible
    environment:
      PM_HOST: '10.0.30.53'
      PM_USER:
        from_secret: proxmox_user
      PM_PASS:
        from_secret: proxmox_pass
      VAULT_PASS:
        from_secret: vault_pass
    settings:
      playbook: .ansible/provision-infra-web.yml
      requirements: .ansible/requirements.txt
      inventory: .ansible/proxmox_inventory.py
      private_key:
        from_secret: key_web # SSH key of the host playbook is targeting
      vault_password:
        from_secret: vault_pass
      tags:
        - ansible-tags-to-run
    when:
      event:
        - push
      branch:
        - master
  ```
  To find out what secrets are available, see [provision-infra-web.yml](https://github.com/UCCNetsoc/NaC/blob/a570cabc13247d846632a6794917e173157dd583/provision-infra-web.yml#L847)
  
  
---
FYI, we are currently investigating cleaner alternatives for CD, such as [Ansible Semaphore](https://ansible-semaphore.com/)
