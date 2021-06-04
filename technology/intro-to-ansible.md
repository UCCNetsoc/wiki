---
title: Intro to Ansible
description: For new and returning SysAdmins
published: true
date: 2021-04-15T15:48:52.489Z
tags: 
editor: undefined
---

[Ansible](https://www.ansible.com/overview/how-ansible-works) is an incredibly powerful IT automation tool we use to configure and install software on servers

* Servers to target are defined in a file (normally called `hosts`, or by a script), and given aliases and groups.
  * Our `hosts` looks a little like this:
    ```
    control

    [proxmox_master]
    feynman

    [proxmox_hosts]
    feynman
    lovelace
    ```
  * These hosts & groups can have `host_vars` and `group_vars` which apply to any server / server in that group respectively
    * Here's an example of `host_vars` for `feynman` (`host_vars/feynman.yml`):
      ```
      ansible_host: 10.0.10.53
      ansible_ssh_user: root
      ansible_ssh_private_key_file: "/root/.ssh/id_rsa"
      ansible_python_interpreter: "/usr/bin/python3"
      ```
    * Here's an example of `group_vars` for a group called `vm` (`group_vars/vm.yml`):
      ```
      ansible_ssh_common_args: '-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
      ansible_python_interpreter: "/usr/bin/python3"
      ansible_user: netsoc
      ```
    * Not only can you make use of explicit variables specify info about the host for Ansible, you can specify your own and access them later
* A _playbook_ consists of a list of _plays_ to run on a host / group of hosts
    * An _play_ could be a bunch of _tasks_ or _roles_
      * A _task_ is a call to an Ansible _module_ that does some functionality
        * Example, to install the `jq` package:
          ```
            apt:
              name: jq
              state: latest
              update_cache: yes
          ```
        * It's important to keep the Ansible module documentation open
          * [Example](https://docs.ansible.com/ansible/latest/modules/apt_module.html)
        * There are modules for *almost everything*, before implementing functionality using a script or shell commands try and see if it can be done using an existing module
        * [An example list of some of the most common modules you may use](https://opensource.com/article/19/9/must-know-ansible-modules)
      * A _role_ is a grouping of functionality 
        * It has lists of tasks, files, variables that can be specified when the role is ran
        * Think of it almost like a script that accepts arguments
        * Example of a role that installs and starts nginx:
        ```
          roles/nginx/vars/defaults.yml:
            start_on_boot: no

          roles/nginx/tasks/main.yml:
          - name: Install nginx package
            apt:
              name: nginx
              state: latest
              update_cache: yes
          
          - name: Start Nginx service (and enable service start on boot if specified)
            service:
              name: nginx
              enabled: "{{ start_on_boot }}"
              state: started
        ```
      * You could call the previous role as a _play_ in a playbook like so:
        ```
        - name: "Ensure nginx on lovelace"
          hosts: lovelace
          roles:
            - role: nginx
              vars:
                start_on_boot: yes
        ```
          * This is the explicit var syntax that scopes the vars to the role, you can also scope the vars to the play
      * Here is a list of tasks as a _play_:
        ```
        - name: Setup server MOTD
          hosts: portal
          tasks:
            - become: yes
              copy:
                content: "{{ lookup('file', './portal-banner.txt') }}"
                dest: /etc/netsoc-banner
            - become: yes
              copy:
                content: "{{ lookup('file', './portal-motd.txt') }}"
                dest: /etc/netsoc-motd
            - become: yes
              shell: /bin/rm -rf /etc/update-motd.d/*
            - become: yes
              copy:
                content: |
                  #!/bin/sh
                  cat /etc/netsoc-motd
                mode: "0755"
                dest: /etc/update-motd.d/00-netsoc-motd
            - become: yes
              copy:
                content: ""
                dest: /etc/legal
            - become: yes
              lineinfile:
                regex: "^#PrintLastLog"
                line: "PrintLastLog no"
                path: /etc/ssh/sshd_config
            - become: yes
              lineinfile:
                regex: "^#Banner none"
                line: "Banner /etc/netsoc-banner"
                path: /etc/ssh/sshd_config
            - become: yes
              service:
                name: sshd
                state: restarted
        ```
        * Don't forget that tasks run after roles if you use them together in a play
      * Ansible supports Jinja variable interpolation almost everywhere
        * Don't forget to look at [filters](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#list-filters)
  * You can run a playbook by doing `ansible-playbook -i hosts playbook.yml`
  * The playbook functionality is a complete IaC addition to Ansible, but you don't actually need to use it
    * Ping every server: `ansible -i hosts all -m ping`
    * Chmod a.txt to 600:  `ansible -i hosts all -m file -a "dest=/srv/foo/a.txt mode=600"`
* Ansible also has a _vault_ for secrets
  * This is like a yaml file full of variables we don't reveal, you can edit the vault and when you finish editing it Ansible will encrypt the file
  * This lets us store our secrets in GitHub but not reveal them to anyone
  * You can edit the vault by doing `ansible-vault edit vars/secrets.yml`
  * When you run a playbook, you can enable vault by doing `ansible-playbook -i hosts playbook.yml --ask-vault-pass`
  * This will decrypt every vault variable and prefix them with `vault_`
  * We normally realias them with the files in `vars/`
    * Example:
    ```
    cloudflare_api_email: "{{ vault_cloudflare_api_email }}"
    cloudflare_api_key: "{{ vault_cloudflare_api_key }}"
    cloudflare_dns_api_token: "{{ vault_cloudflare_dns_api_token }}"
    ```

* **[Keep all operations idempotent, this is the core philosophy to Ansible](https://docs.ansible.com/ansible/latest/reference_appendices/glossary.html#term-idempotency)**