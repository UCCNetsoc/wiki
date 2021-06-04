---
title: Netsoc-as-Code
description: Defining a society one YAML file at a time
published: true
date: 2021-04-15T15:48:35.873Z
tags: 
editor: undefined
---

Since all of Netsoc's services run as VMs on Proxmox, we can define our entire infrastructure using Infrastructure-as-Code. 

[All of our IaC is fully open source and available on GitHub](https://github.com/UCCNetsoc/NaC)

While you read this wiki page, you may find it helpful to check out the files as they're mentioned

# Why do we do this?

### We can use pre-built base images
* Popular distros already have releases of cloud-images for clouds like AWS EC2, GCP & Azure
  	* https://cloud-images.ubuntu.com/
  	* These images typically come locked down, you can only configure them with cloud-init
* Proxmox supports the cloud-init system 
 

### We can define and create VMs at will

* VM creation is done by our custom VM Ansible role `proxmox-cloudinit-vm`:
  * It sets up a Proxmox VM with it's disk set to a clone of the base image
  * It's expected that the disk image we clone has cloud-init & qemu-guest-agent installed
    * [cloud-init](https://cloudinit.readthedocs.io/en/latest/) is the de-facto industry standard for configuring Cloud VMs
      * We use it to inject maintenance user info (ssh keys, etc) and network configuration into our VMs
      * Note, the role we use accepts only _plain-text_ yaml, this is because not every format for userdata only uses yaml. [Look here in the cloud-init docs](https://cloudinit.readthedocs.io/en/latest/topics/format.html) to see what I mean
    * [qemu-guest-agent](https://pve.proxmox.com/wiki/Qemu-guest-agent) is a package that exposes the current network setup and stats to the KVM machine
      * i.e. it let's us see what VMs are _actually_ binding to what static IPs we asked them to
      * also used for some Proxmox related functionality
    * Set specifications:
      * Cores, Memory, Network config
    * Resize existing disks
    * Set new disks
    * Set the description:
      * All our IaC managed VMs are expected have descriptions containing YAML.
      * We use this YAML later to "discover" info about any running VMs
      * Example from (`create-infra-databases.yml`):
        ```yaml
            groups:
              - vm
              - prometheus_base
              - prometheus_docker
              - fluentd
              - ipaclients
            host_vars:
              ansible_ssh_private_key_file: "./keys/infra/databases/id_rsa"
        ```
      * We will use this to pass info about running VMs to Ansible later
    * Supply Cloud-Init metadata / userdata / network config
      * If you're reading cloud-init docs, we use the [NoCloud data source](https://cloudinit.readthedocs.io/en/latest/topics/datasources/nocloud.html)
      * We generally don't use metadata/instance metadata
        * More targeted towards actual clouds like EC2
      * We use userdata to inject the Netsoc maintenance user and set the hostname (using [`#cloud-config`](https://cloudinit.readthedocs.io/en/latest/topics/examples.html))
      * Network config [(we use v2)](https://cloudinit.readthedocs.io/en/latest/topics/network-config-format-v2.html#network-config-v2) is used to tell the VM what static IP it should bind to, what DNS server is should use & what router it should send packets to
      * Example (edited slightly) from (`create-infra-databases.yml`):
          ```yaml
          cloudinit:
            pool: local
            force: yes
            userdata: |
              #cloud-config
              preserve_hostname: false
              manage_etc_hosts: true
              fqdn: databases.infra.netsoc.co
              packages:
                - qemu-guest-agent
              runcmd:
                - [ systemctl, enable, qemu-guest-agent ]
                - [ systemctl, start, qemu-guest-agent, --no-block ]  
              users:
                - name: netsoc
                  gecos: Netsoc Management User
                  primary_group: netsoc
                  groups: netsoc
                  shell: /bin/bash
                  sudo: ALL=(ALL) NOPASSWD:ALL
                  ssh_authorized_keys:
                    - "{{ lookup('file', './keys/infra/databases/id_rsa.pub') }}"
              apt: 
                preserve_sources_list: true
                primary:
                  - arches: [default]
                    uri: http://ie.archive.ubuntu.com/ubuntu/
                security:
                  - uri: http://security.ubuntu.com/ubuntu
              disk_setup:
                /dev/vdb:
                  table_type: 'gpt'
                  layout:
                    - [100,83] # 100% Linux fs
                  overwrite: false
              fs_setup:
                - label: data-disk
                  filesystem: ext4
                  device: '/dev/vdb1'
                  overwrite: false
              mounts:
                - [ vdb, /netsoc, auto ]
            networkconfig: 
              version: 2
              ethernets: "{{ interfaces.infra.databases }}"
          ```
     * This cloud-init file does the following:
      	* Sets the hostname & fully-qualified-domain-name (FQDN) of the machine
        * Installs `qemu-guest-agent` from APT repos
        * Enables and starts the `qemu-guest-agent` service
        * Uses Irish APT repos for speed over the base Ubuntu ones
        * Installs a `netsoc` user with `sudo` permissions and a public key so we can ssh in to provision the VM
        * Sets the partition table the 2nd disk to be 100% Linux FS (GPT)
        * Partitions the disk with ext4 
        * Mounts it at /netsoc
        * Network-config sets the IP address, gateway and other network information
  * See `create-infra-auth.yml` for more info

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
  * i.e if the vm is databases.infra.netsoc.co, install MySQL
  * They target the groups we specified in the description of the VM
    * Don't forget you can target by name too, like `databases.vm.netsoc.co`

### TODO(ocanty) >not yet< We can completely define our router config without the use of messy web UIs like pfsense
  * We have more VMs than we have public IP addresses, therefore we must make use of a router and a [NAT](https://www.reddit.com/r/explainlikeimfive/comments/1wqc30/eli5_how_does_nat_network_address_translation_work/) configuration
  * We do not use a hardware router, instead we use a virtualized router called [VyOS](https://www.vyos.io/)
  * We can map any port on any of our available IP addresses to any port on a VM
  * We can use VLANs to seperate infra and user server traffic
  * All of this is configured via Ansible
  * See `provision-router.yml`

### We can manage our DNS records
  * All (external) DNS records are hosted on Cloudflare and completely managed by the `setup-external-dns.yml` playbook
  * Likewise, internal DNS is handled by FreeIPA and managed using `provision-infra-auth-internal-dns.yml`

### All of this should tie into itself
  * We use `vars/network.yml` to set IP allocations for VMs
  * We can feed that to `create-*.yml` to create a VM that uses that IP
  * We can then provision it using it's respective provision playbook
  * We can then set a DNS record to point at the public IP if we want requests to hit that VM externally

### All of this can be managed with git and GitHub
* Issues & pull requests used to manage infrastructure
* Permanent history of Netsoc infra for years to come
* Everything is centralized, a single PR can update every part of the infra rather than being done manually by hand
* Repo is (relatively) self-documenting

# Full Example

## Define it's network

`vars/network.yml` excerpt
```yaml
vlan:
  wan:     10 
  proxmox: 20 
  infra:   30 
  k8s:     40 
  cloud:   50 
  extern:  60  # VRRP stuff
  router:  70  # Used for vyos clustering + failover
  oob:     80  # OOB, connected to CIX OOBnet and OOB ports on machines
  mgmt:    90  #

subnets:
  wan:      "10.0.{{ vlan.wan }}.0/24"
  proxmox:  "10.0.{{ vlan.proxmox }}.0/24"
  infra:    "10.0.{{ vlan.infra }}.0/24"
  k8s:      "10.{{ vlan.k8s }}.0.0/16"
  cloud:    "10.{{ vlan.cloud }}.0.0/16"

gateway4:
  infra:    "10.0.{{ vlan.infra }}.1"
  k8s:      "10.{{ vlan.k8s }}.0.1"
  
nameservers:
  infra:
    search:
      - "infra.netsoc.co"
    addresses: 
      - "{{ subnets.infra | ipmath(15) }}" # 10.0.30.15!
  public:
    search: []
    addresses:
      - 1.1.1.1

vmbr: "vmbr0"

interfaces:
  infra:
    databases:
      net0:
        match:
          macaddress: "b2:eb:14:0a:54:a5"
        addresses:
          - "{{ subnets.infra | ipaddr(25) }}" # 10.0.30.25!
        gateway4: "{{ gateway4.infra }}"
        nameservers: "{{ nameservers.infra }}"
        optional: true
        link-local: []
```

## Define the specifications and Cloud-Init

* Cloud-Init is used to partition an additional 'data disk' and mount it on `/netsoc`
* That means we only have to backup the data disk instead of the boot drive

`create-infra-databases.yml`

```yaml
- name: "INFRA - Ensure databases server"
  hosts: lovelace
  roles:
    - role: proxmox-cloudinit-vm
      vars:
        vm:
          name: "databases.infra.netsoc.co"
          clone:
            pool: local
            qcow2_url: "https://cloud-images.ubuntu.com/releases/focal/release-20200921.1/ubuntu-20.04-server-cloudimg-amd64.img"
            qcow2_hash: sha256:d18a9d2b890d3c1401e70a281cae44b61816aa669c4936f7a99c168e572ec8cb
          cores: 4
          memory: 4096
          description:
            groups:
              - vm
              - prometheus_base
              - prometheus_docker
              - fluentd
              - ipaclients
            host_vars:
              ansible_ssh_private_key_file: "./keys/infra/databases/id_rsa"
          net:
            net0: "virtio={{ interfaces.infra.databases.net0.match.macaddress }},tag={{ vlan.infra }},bridge={{ vmbr }}"
          disks:
            pool: local-lvm
            boot:
              size: 30G
              format: raw
            extra:
              - size: 45G
                format: raw
          efi:
            pool: local
          cloudinit:
            pool: local
            force: yes
            userdata: |
              #cloud-config
              preserve_hostname: false
              manage_etc_hosts: true
              fqdn: databases.infra.netsoc.co
              packages:
                - qemu-guest-agent
              runcmd:
                - [ systemctl, enable, qemu-guest-agent ]
                - [ systemctl, start, qemu-guest-agent, --no-block ]  
              users:
                - name: netsoc
                  gecos: Netsoc Management User
                  primary_group: netsoc
                  groups: netsoc
                  shell: /bin/bash
                  sudo: ALL=(ALL) NOPASSWD:ALL
                  ssh_authorized_keys:
                    - "{{ lookup('file', './keys/infra/databases/id_rsa.pub') }}"
              apt: 
                preserve_sources_list: true
                primary:
                  - arches: [default]
                    uri: http://ie.archive.ubuntu.com/ubuntu/
                security:
                  - uri: http://security.ubuntu.com/ubuntu
              disk_setup:
                /dev/vdb:
                  table_type: 'gpt'
                  layout:
                    - [100,83] # 100% Linux fs
                  overwrite: false
              fs_setup:
                - label: data-disk
                  filesystem: ext4
                  device: '/dev/vdb1'
                  overwrite: false
              mounts:
                - [ vdb, /netsoc, auto ]
            networkconfig: 
              version: 2
              ethernets: "{{ interfaces.infra.databases }}"
          wait_for_ssh_ip: "{{ interfaces.infra.databases.net0.addresses[0] }}"
  vars_files:
    - vars/network.yml
    - vars/secrets_mapping.yml
    - vars/secrets.yml

- name: "Reload inventory to pull new VMs"
  hosts: 127.0.0.1
  connection: local
  tasks:
    - meta: refresh_inventory
```

Then run `./start-dev.sh` to get into dev mode followed by `./run.sh create-infra-databases.sh`

`run.sh` is a handy script that hides all the `ansible-playbook` flags you need

## Setup the databases, install Postgres & MySQL!

`provision-infra-databases.yml`
```yaml
- name: "INITIAL SETUP"
  hosts: databases.infra.netsoc.co
  become: yes
  roles:
    - ansible-requirements
    - utf8-locale
    - docker
  tasks:
    - name: "Make /netsoc only readable by root"
      file:
        path: "/netsoc"
        owner: root
        group: root
        mode: '1770'
  tags:
    - initial


- name: "DATABASES - MySQL/PG"
  hosts: databases.infra.netsoc.co
  become: yes
  tasks:    
    - name: MYSQL/PG/PROM - Ensure directories exist
      file:
        path: "/netsoc/{{ item }}"
        owner: root
        group: root
        # Needs to be 775 as this folder is accessed internally by a dirsrv user
        # https://github.com/freeipa/freeipa-container/issues/281
        mode: '0755'
        state: directory
      with_items:
        - mysql
        - pg

    - name: PG - Install psycopg2 (needed for ansible pg driver)
      apt:
        name: python3-psycopg2
        state: present

    - name: MYSQL/PG - Remember Compose definition
      set_fact:
        definition: 
          version: "3.5"
          services:
            mysql:
              hostname: "mysql.databases.infra.netsoc.co"
              image: mysql:8.0.20
              restart: on-failure
              environment:
                MYSQL_ROOT_PASSWORD: "{{ mysql.infra.root_password }}"
              volumes:
                - "/netsoc/mysql:/var/lib/mysql"
              ports:
                - "3306:3306"
            pg:
              hostname: "pg.databases.netsoc.co"
              image: postgres:12.3
              restart: on-failure
              environment:
                POSTGRES_PASSWORD: "{{ postgres.infra.postgres_password }}"
              volumes:
                - "/netsoc/pg:/var/lib/postgresql/data"
              ports:
                - "5432:5432"

    - name: MYSQL/PG - Setup services
      docker_compose:
        project_name: databases
        recreate: smart
        remove_orphans: yes
        state: present
        definition: "{{ definition }}"
  vars_files:
    - "vars/network.yml"
    - "vars/secrets_mapping.yml"
    - "vars/secrets.yml"
  tags:
    - provision
```

Then run `./run.sh provision-infra-databases.sh`

If you wanted to run this again but recreate the database images you can run `./run.sh provision-infra-databases.sh --tags provision` to only select the tasks with `provision` tags
	* The data already stored in the dbs won't be destroyed because they mount the folders already on the data disk),
