Table of contents

1. [What is ansible ?](#what-is-ansible)
2. [How is ansible agentless ?](#ansible-is-agentless)
3. [How is ansible modules built?](#how-is-ansible-modules-built?)
4. [What is Playbooks in Ansible ?](#what-is-playbooks-in-ansible)
5. [What is Inventories in Ansible ?](#what-is-inventories-in-ansible)
6. [What is Ansible navigator ?](#what-is-ansible-navigator)
7. [What is Ansible navigator ?](#what-is-ansible-navigator)
8. [How to configure ansible configuration ?](#how-to-configure-ansible-configuration)


## what-is-ansible
Ansible is an open-source automation tool used in linux for Configuration management, Application deployment, Provisioning, Orchestration.

## ansible-is-agentless
Ansible is agentless meaning we don't have install it on all the server ( child nodes - All linux os'es ) that we are goingto manage operations against.

It only needs to installed on the control node ( main node ) where we write the ansible playbook to control the child nodes.

## how-is-ansible-modules-built?
Ansible modules are mainly built using the python.  Most of the implementations we don't have to worry about.  It's ready to use.  We just have to use the modules in the tasks section or any other sections in the ansible playbooks.

## what-is-playbooks-in-ansible
Ansible playbooks are YAML files that define a series of automation tasks to be executed on remote systems.

Eg of Ansible playbooks

```
---
- name: Install and start nginx
  hosts: webservers
  become: true

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx service
      service:
        name: nginx
        state: started
```

## what-is-inventories-in-ansible

An inventory defines a collection of hosts that Ansible manages. These hosts can also be assigned to groups, which can be managed collectively. Groups can contain child groups, and hosts can be members of multiple groups. The inventory can also set variables that apply to the hosts and groups that it defines.

## what-is-ansible-navigator
Ansible Navigator is a command-line tool used to run, debug, and explore Ansible automation content in a more interactive way.

It acts as a modern interface for working with:

- Playbooks
- Collections
- Inventories
- Execution Environments (containers with Ansible preinstalled)

## how-to-configure-ansible-configuration
You can create an ansible.cfg file in your Ansible project's directory to apply settings that apply to multiple Ansible tools.

The Ansible configuration file consists of several sections, with each section containing settings defined as key-value pairs. Section titles are enclosed in square brackets.

For example:

```
[defaults]
inventory = ./inventory
remote_user = user
ask_pass = false

[privilege_escalation]
become = true
become_method = sudo
become_user = root
become_ask_pass = false
```

