---
title: Netsoc-as-Code
description: Defining a society one YAML file at a time
published: true
date: 2020-07-14T02:23:24.270Z
tags: 
editor: undefined
---

Since all of Netsoc's services run as VMs on Proxmox, we can define our entire infrastructure using Infrastructure-as-Code. 

[All of our IaC is fully open source and available on GitHub](https://github.com/UCCNetsoc/NaC)

While you read this wiki page, you may find it helpful to check out the files as they're mentioned

# Why do we do this?

### We can define and create a "template" VM that contains pre-installed tools and a base image
  * Template VMs are created using [Hashicorp Packer](https://www.packer.io)
  * We install the distro OS and then do further provisioning via Ansible/shell scripts
  * We do not call the Packer executable directly, we instead use an Ansible role we created called `packer-proxmox-template`
    * Packer doesn't support saving the VM template to a distributed storage
      * Not that we're using one, yet
    * For a given template i.e `netsoc-centos-6`, we will first build the template on a specified server under the name `netsoc-centos-6-<hostname>`
    * The template is then dumped and copied to every other Proxmox server and hostname renamed
    * When we want to create instances of the template, we clone the template on that server
    * i.e for `netsoc-ubuntu-server`, we have `netsoc-ubuntu-server-lovelace` and `netsoc-ubuntu-server-feynman`
  * See `template-netsoc-ubuntu-server.yml`

### We can define and create VMs at will
  * VM creation is done by our custom VM Ansible role `proxmox-infra-cloudinit-vm`
  * We create a clone of a template VM defined previously
  * It's expected that the template VM we cloned has cloud-init & qemu-guest-agent installed
    * [cloud-init](https://cloudinit.readthedocs.io/en/latest/) is the de-facto industry standard for configuring Cloud VMs
      * We use it to inject maintenance user info (ssh keys, etc) and network configuration into our VMs
      * Note, the role we use accepts only _plain-text_ yaml, this is because not every format for userdata only uses yaml. [Look here in the cloud-init docs](https://cloudinit.readthedocs.io/en/latest/topics/format.html) to see what I mean
    * [qemu-guest-agent](https://pve.proxmox.com/wiki/Qemu-guest-agent) is a package that exposes the current network setup and stats to the KVM machine
      * i.e. it let's us see what VMs are _actually_ binding to what static IPs we asked them to
      * also used for some Proxmox related functionality
  * We then modify the clone we created
    * Set specifications:
      * Cores, Memory, Network config
    * Resize existing disks
    * Set new disks
      * **If you rerun the playbook, we will only recreate the disks if you explicitly say we should**
        * Prevents loss of user data
    * Set the description:
      * All our IaC managed VMs are expected have descriptions containing YAML.
      * We use this YAML later to "discover" info about any running VMs
      * Example from (`create-auth.yml`):
        ```yaml
        groups:
          - vm
          - auth
        host_vars:
          ansible_ssh_private_key_file: ./keys/auth/id_rsa
        ```
      * We will use this to pass info about running VMs to Ansible later
    * Supply Cloud-Init metadata / userdata / network config
      * If you're reading cloud-init docs, we use the [NoCloud data source](https://cloudinit.readthedocs.io/en/latest/topics/datasources/nocloud.html)
      * We generally don't use metadata/instance metadata
        * More targeted towards actual clouds like EC2
      * We use userdata to inject the Netsoc maintenance user and set the hostname (using [`#cloud-config`](https://cloudinit.readthedocs.io/en/latest/topics/examples.html))
      * Network config [`(we use v2)`](https://cloudinit.readthedocs.io/en/latest/topics/network-config-format-v2.html#network-config-v2) is used to tell the VM what static IP it should bind to, what DNS server is should use & what router it should send packets to
      * Example (edited slightly) from (`create-auth.yml`):
        ```yaml
        cloudinit:
          drive_device: ide2
          metadata: ""
          userdata: |
            #cloud-config
            users:
              - name: netsoc
                gecos: Netsoc Management User
                primary_group: netsoc
                sudo: ALL=(ALL) NOPASSWD:ALL
                ssh_authorized_keys:
                  - {{ lookup('file', './keys/auth/id_rsa.pub') }}
            preserve_hostname: false
            manage_etc_hosts: true
            fqdn: auth.vm.netsoc.co
          networkconfig:
            version: 2
            ethernets:
              30a:
                match:
                  name: ens18
                addresses:
                  - 10.0.30.10/24
                gateway4: 10.0.30.1
                nameservers:
                  search: [vm.netsoc.co]
                  addresses: [10.0.30.20]
        ```
      * `drive_device` specifies what device the NoCloud CD-ROM drive is mounted on
  * See `create-auth.yml` for more info

### We need to be able to discover every VM we have running in order to customize the base image we cloned further
  * Proxmox has an API
  * Description field was set with our previous Ansible-related groups and vars
  * `qemu-guest-agent` will tell us what IPs the machines are running
  * Ansible has a feature known as [custom inventory scripts](https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html#dynamic-inventory)
    * Ansible normally searches for machines to connect to in predefined files (i.e ./hosts, ./hosts.yml, ansible.cfg, etc...)
    * A custom inventory script is a script that instead can be called to return the list of possible machines to connect to
    * It's really common to use with Cloud providers (i.e discover every AWS EC2 instance you have running and run a command on them all)
    * Our custom inventory script for Proxmox is `proxmox_inventory.py`
      * Before you can use it, you must have loaded the environmental variables from the Ansible vault (these contain the API password)
      * You can do this by running the `./start-dev.sh` script
    * You can now see we can set `host_vars` from the VM's description
    * This is how we discover which ssh key in the NaC directory to use, and other info
    * The inventory script will now work, try `ansible -i proxmox_inventory.py all -m ping` and it should be able to ping all the VMs
    * Similarly you will see that `ansible-playbook -i proxmox_inventory.py ...` will use that inventory
    * You can target the cluster hosts at the same time too by using `ansible-playbook -i hosts -i proxmox_inventory ...`
    * If you want to get the json we passed to Ansible for yourself, you can run `./proxmox_inventory.py --list`

### We can then further provision that VM with tools that only it specifically needs
  * See `provision-*.yml`
  * i.e if the vm is databases.vm.netsoc.co, install MySQL
  * They target the groups we specified in the description of the VM
    * Don't forget you can target by name too, like `databases.vm.netsoc.co`

### We can completely define our router config without the use of messy web UIs like pfsense
  * We have more VMs than we have public IP addresses, therefore we must make use of a router and a [NAT](https://www.reddit.com/r/explainlikeimfive/comments/1wqc30/eli5_how_does_nat_network_address_translation_work/) configuration
  * We do not use a hardware router, instead we use a virtualized router called [VyOS](https://www.vyos.io/)
  * We can map any port on any of our available IP addresses to any port on a VM
  * We can use VLANs to seperate infra and user server traffic
  * All of this is configured via Ansible
  * See `provision-router.yml`

### We can manage our DNS records
  * All (external) DNS records are hosted on Cloudflare and completely managed by the `setup-external-dns.yml` playbook
  * Likewise, internal DNS is handled by FreeIPA and managed using `setup-internal-dns.yml`

### All of this should tie into itself
  * We use `vars/network.yml` to set IP allocations for VMs
  * We can feed that to `create-*.yml` to create a VM that uses that IP
  * We can feed that to `provision-router.yml` and create a port-mapping that maps an external port to that VM's IP 
  * We can then set a DNS record to point at the public IP we use that the external port is on

### All of this can be managed with git and GitHub
* Issues & pull requests used to manage infrastructure
* Permanent history of Netsoc infra for years to come
* Everything is centralized, a single PR can update every part of the infra rather than being done manually by hand
* Repo is (relatively) self-documenting

# Full Example

`vars/network.yml` excerpt
```yaml
interfaces:
  auth:
    net0:
      addresses:
        - "10.0.{{ subnets.infra }}.10/24"
      gateway4: "{{ gateway4.infra }}"
      nameservers: *nameservers
      optional: true # prevents waiting for network hang
      link-local: [ ipv4 ]
```

`create-auth.yml`

```yaml
- name: "Ensure auth server"
  hosts: lovelace
  roles:
    - role: proxmox-infra-cloudinit-vm
      vars:
        vm:
          name: "auth.vm.netsoc.co"
          clone: "netsoc-ubuntu-server-{{ inventory_hostname }}"
          net:
            net0: "virtio,bridge=vmbr0,tag={{ vlans.infra }}"
          disks:
            virtio0:
              resize: "20G"
            virtio1:
              pool: local-lvm
              definition: "25,format=raw"
          cores: 4
          memory: 2048
          description:
            groups:
              - vm
              - auth
              - ipaclients
            host_vars:
              ansible_ssh_private_key_file: "./keys/auth/id_rsa"
        cloudinit:
          drive_device: ide2
          userdata: |
            #cloud-config
            users:
              - name: netsoc
                gecos: Netsoc Management User
                primary_group: netsoc
                sudo: ALL=(ALL) NOPASSWD:ALL
                ssh_authorized_keys:
                  - {{ lookup('file', './keys/auth/id_rsa.pub') }}
            preserve_hostname: false
            manage_etc_hosts: true
            fqdn: auth.vm.netsoc.co
          networkconfig: 
            version: 2
            ethernets:
              ens18: "{{ interfaces.auth.net0 }}"
        wait_for_ssh_ip: "{{ interfaces.auth.net0.addresses[0] }}"
  vars_files:
    - vars/network.yml
    - vars/proxmox.yml
    - vars/secrets.yml

- name: "Reload inventory to pull new VMs"
  hosts: 127.0.0.1
  connection: local
  tasks:
    - meta: refresh_inventory
```

`provision_auth.yml`
```yaml
- hosts: auth
  become: yes
  tasks:
    # Because the Auth server also hosts the FreeIPA DNS server
    # We configured it in create-auth.yml with cloudinit network config to use itself as it's DNS server
    # However we haven't created the FreeIPA installation yet so we need to use a temporary nameserver
    - shell: |
        systemctl stop systemd-resolved
        systemctl disable systemd-resolved
    - copy:
        content: |
          nameserver 1.1.1.1
        dest: /etc/resolv.conf

- name: "Ensure IPA server disk mounted and IPA server created"
  hosts: auth
  become: yes
  roles:
    - role: simple-disk
      vars:
        device: "/dev/vdb"
        partition_size: "100%"
        fstype: "ext4"
        mount_path: "/mnt/data-disk"
        mount_opts: "rw"
    - role: freeipa-server-docker
      vars:
        mount:             "/mnt/data-disk/freeipa"
        interface_address: "{{ interfaces.auth.net0.addresses[0] }}"
  vars_files:
    - "vars/freeipa.yml"
    - "vars/network.yml"
    - "vars/secrets.yml"

- hosts: auth
  become: yes
  tasks:
    # Restore DNS to point at FreeIcaPA localhost DNS
    # allows cloud-init to remain working 
   - copy:
        content: |
          nameserver {{ interfaces.auth.net0.addresses[0] | ipaddr('address') }}
        dest: /etc/resolv.conf
  vars_files:
    - "vars/network.yml"

- name: "Enroll in IPA server"
  hosts: auth 
  roles:
    - role: freeipa-client
      vars:
        client:
          addresses: 
            - "{{ ansible_default_ipv4.address }}"
  vars_files:
    - "vars/freeipa.yml"
    - "vars/network.yml"
    - "vars/secrets.yml"

- name: "Setup Keycloak"
  hosts: auth
  roles:
    - role: keycloak-docker
      vars:
        mount: "/mnt/data-disk/keycloak"
        realm: "{{ keycloak_freeipa_realm }}"
  vars_files:
    - "vars/keycloak.yml"
    - "vars/keycloak_freeipa_realm.yml"
    - "vars/freeipa.yml"
    - "vars/secrets.yml"
```
