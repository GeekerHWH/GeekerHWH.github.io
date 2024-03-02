---
title: "Ansible Learning Notes"
date: 2024-02-24T15:18:55+08:00
draft: false
author: "GeekerHWH"
category:
    - Technology
tags:
    - DevOps
    - Automation
    - Linux
image: /images/blogs/Ansible.webp
description: ""
toc: true
---

## Prefix
This page is my own notes during learning Ansible, and can't be alternative to **[official documents](https://docs.ansible.com/)**. All knowledge comes from Udemy course: **[Ansible for the Absolute Beginner - Hands-On - DevOps](https://www.udemy.com/course/learn-ansible/?couponCode=KEEPLEARNING)**

### Why Ansible
the usages of ansible:
- Provisioning
- Configuration Management
- Continuous Delivery / Continuous Integration
- Application Deployment
- Security Compliance

### Ansible vs Scripts
| scripts       | Ansible   |
| ------------- | --------- |
| Time          | Simple    |
| Coding Skills | Powerful  |
| Maintenance   | Agentless |

> "Agentless" means you don't need to install any agent software in the target machines to work with ansible.

## setup
set up ansible controller
```bash
vim /etc/hostname
vim /etc/hostfile
```

set up ansible targets 1 and 2

## install
debian as an example, for other distros please check the [Instruction for Installing](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html)
```bash
sudo apt install ansible
# ansible --version # check if installation was completed
```

## inventoy
inventoy is used for configuration for the controller, telling it how to connect
to the targets

default address: /etc/ansible/host

```ini
[web_servers]
master_server ansible_host=master.server.com
slave_server ansible_host=192.168.1.11

[db_servers]
db1 ansible_host=192.168.1.20
```

### Basic parameters
Inventory Parameters:
- ansible_host: the domain or ip addr of the host
- ansible_connection: connection protocol (e.g. ssh/winrm/localhost)
- ansible_port: connection port 22/5986
- ansible user: username root/administrator
- ansible_ssh_pass: SSH Password

## Commands
```bash
ansible target1 -m ping -i inventory.txt # -m module -i input
ansible <host> -a <command>
ansible <host> -m <mmodule>
```

## ansible config
default address: /etc/ansible/ansible.cfg

## Ansible Playbooks
Playbook is a single YAML file
- Play: Defines set of activities(tasks) to be run on hosts
    - Task: An action to be performed on the host
        - Execute a command
        - Run a script
        - Install a package
        - Shutdown/Restart

```bash
ansible-playbook <playbook_path> -i <inventoy_name>
```

## Modules
- System
  - User
  - Group
  - Hostname
  - Iptables
  - Lvg
  - Lvol
  - Make
  - Mount
  - Ping
  - Timezone
  - Systemd
  - Service
- Commands
  - Command
  - Expect
  - Raw
  - Script
  - Shell
- Files
  - Acl
  - Archive
  - Copy
  - File
  - Find
  - Lineinfile
  - Replace
  - Stat
  - Template
  - Unarchive
- Database
  - Mongodb
  - Mssql
  - Mysql
  - Postgresql
  - more...
- Cloud
  - Amazon
  - Atomic
  - Azure
  - Centrylink
  - more...
- Windows
  - Win_copy
  - Win_command
  - Win_domain
  - Win_file
  - more...
- more...

Idempotency: An operation is idempotent if the result of performing it once is exactly the same as the result of performing it repeatedly without any intervening actions.

## Variables
stores the information that varies with each host.

you can define variables in a file with same name of the host

single variable should be included in `'`, e.g.
- '{{variable}}'
- somthing {{variable}} somthing
## Conditionals
check examples
## Loops
check examples

### Examples
```yml
# simple variable use case
-
    name: Add DNS server to resolv.conf
    hosts: localhost
        vars:
            dns_server: 10.1.250.10
        tasks:
            - lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver {{ dns_server }}'
```

```yml
# simple conditional use case
-
  name: Install NGINX
  hosts: all
  tasks: 
  - name: Install NGINX on Debian
    apt:
      name: nginx
      state: present
    when: ansible_os_family == "Debian"   and
          ansible_distribution_version == "16.04"
  - name: INstall NGINX on Redhat
    yum:
      name: nginx
      state: present
    when: ansible_os_family == "RedHat"   or
          ansible_os_family == "SUSE"
```

```yml
# conditional in loops
-
  name: Install Softwares
  hosts: all
  vars:
    packages:
      - name: nginx
        required: True
      - name: mysql
        required: True
      - name: apache
  tasks:
  - name: Install "{{ item.name }}" on Debian
    apt:
      name: "{{ item.name }}"
      state: present
    
    when: item.required == True
    loop: "{{ packages }}"
```

```yml
# conditional and register
-
  name: Check status of httpd and email if its down
  hosts: all
  tasks:
    - command: service httpd status
      register: result

    - mail:
        to: admin@company.com
        subject: HTTPD Service Alert
        body: Httpd Service is down
        when: result.stdout.find('down') != -1
```

```yml
#loops
-
  name: Create users
  hosts: localhost
  tasks:
  - user: name= '{{item.name}}' state=present uid= '{{irem.uid}}'
    loop:
      - name: joe
        uid: 1010
      - name: george
        uid: 1011
      - name: mike
        uid: 1012
```

## Ansible Role
the primary purpose of role is to make your tasks code reusable