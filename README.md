Table of contents

1. [What is ansible ?](#what-is-ansible)
1. [How is ansible agentless ?](#ansible-is-agentless)
1. [How is ansible modules built?](#how-is-ansible-modules-built?)
1. [Comments-in-ansible](#comments-in-ansible)
1. [What is Playbooks in Ansible ?](#what-is-playbooks-in-ansible)
1. [What is Inventories in Ansible ?](#what-is-inventories-in-ansible)
1. [What is Ansible navigator ?](#what-is-ansible-navigator)
1. [What is Ansible navigator ?](#what-is-ansible-navigator)
1. [How to configure ansible configuration ?](#how-to-configure-ansible-configuration)
1. [What is ansible facts ?](#what-is-ansible-facts)
1. [Variables in ansible playbook](#variables-in-ansible-playbook)
1. [Variable precedence in Ansible](#variables-precedence-in-ansible)
1. [Loops in Ansible](#loops-in-ansible)
1. [Conditional tasks in Ansible](#conditional-tasks-in-ansible)
1. [Ignoring task failure in Ansible](#ignoring-task-failure-in-ansible)
1. [What is jinja2 in Ansible](#what-is-jinja2-in-ansible)
1. [How is jinja2 used in Ansible](#how-is-jinja2-used-in-ansible)
1. [Why Ansible Handlers are idempotent](#why-ansible-handlers-are-idempotent)
1. [What is ansible tower](#what-is-ansible-tower)



## what-is-ansible
Ansible is an open-source automation tool used in linux for Configuration management, Application deployment, Provisioning, Orchestration.

## ansible-is-agentless
Ansible is agentless meaning we don't have install it on all the server ( child nodes - All linux os'es ) that we are goingto manage operations against.

It only needs to installed on the control node ( main node ) where we write the ansible playbook to control the child nodes.

## how-is-ansible-modules-built?
Ansible modules are mainly built using the python.  Most of the implementations we don't have to worry about.  It's ready to use.  We just have to use the modules in the tasks section or any other sections in the ansible playbooks.

## comments-in-ansible
In ansible comments can be added to the file using '#' sign.

eg:
```
#This is a comment
```

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

## what-is-ansible-facts
In Ansible, facts are pieces of information automatically collected about a managed host before tasks run.

eg.  OS type, IP address, CPU details, Memory, Hostname, Network interfaces etc.

## variables-in-ansible-playbook
In Ansible, variables are used to store values that can be reused throughout a playbook, making your automation more flexible and maintainable.

eg:
```
---
- name: Install and start Apache
  hosts: webservers
  vars:
    package_name: httpd
    service_name: httpd

  tasks:
    - name: Install package
      yum:
        name: "{{ package_name }}"
        state: present

    - name: Start service
      service:
        name: "{{ service_name }}"
        state: started
```

## variables-precedence-in-ansible

The below table illustrate the variable precedence in ansible
| Precedence (Low → High) | Source |
|-------------------------|--------|
| 1 | Role defaults (`roles/<role>/defaults/main.yml`) |
| 2 | Inventory group vars |
| 3 | Inventory host vars |
| 4 | Playbook `group_vars/all` |
| 5 | Playbook `group_vars/*` |
| 6 | Playbook `host_vars/*` |
| 7 | Facts gathered by the setup module |
| 8 | Play vars |
| 9 | Play `vars_prompt` |
| 10 | Play `vars_files` |
| 11 | Role vars (`roles/<role>/vars/main.yml`) |
| 12 | Block vars |
| 13 | Task vars |
| 14 | `include_vars` |
| 15 | `set_fact` and registered vars |
| 16 | Role/include params |
| 17 | Extra vars (`ansible-playbook -e`) |

## loops-in-ansible

Basic example of using loops in ansible:
```
vars:
  mail_services:
    - postfix
    - dovecot
tasks:
  - name: Postfix and Dovecot are running
    ansible.builtin.service:
      name: "{{ item }}"
      state: started
    loop: "{{ mail_services }}"
```
## conditional-tasks-in-ansible

Basic example of using conditional tasks in ansible.
```
---- name: Simple Boolean Task Demo
  hosts: all
  vars:
    run_my_task: true
  tasks:
    - name: httpd package is installed
      ansible.builtin.dnf:
        name: httpd
      when: run_my_task
```

## ignoring-task-failure-in-ansible
By default, if a task fails, the play is aborted. However, this behavior can be overridden by ignoring failed tasks. You can use the ignore_errors keyword in a task to accomplish this.

The below example shows how a task failure can be handled in ansible.
```
- name: Latest version of notapkg is installed
  ansible.builtin.dnf:
    name: notapkg
    state: latest
ignore_errors: yes
```

## what-is-jinja2-in-ansible
Jinja2 is a fast, expressive, and highly extensible template engine for the Python programming language.

## how-is-jinja2-used-in-ansible
Ansible uses the Jinja2 templating engine to dynamically evaluate expressions, manipulate data, and generate configuration files.

when used inside playbook yaml file:
```
- name: Print host info
  ansible.builtin.debug:
    msg: "Target hostname is {{ ansible_facts['hostname'] }}"
```

when used inside template file:
```
# Generated by Ansible on {{ ansible_date_time.date }}

[server]
listen_port = {{ web_port | default(80) }}

{% if enable_debug %}
log_level = DEBUG
{% else %}
log_level = INFO
{% endif %}

[backends]
{% for server in backend_servers %}
server_ip = {{ server }}
{% endfor %}
```

## why-ansible-handlers-are-idempotent
Ansible modules are designed to be idempotent. This means that if you run a playbook multiple times, the result is always the same. You can run plays and their tasks multiple times, but managed hosts are only changed if those changes are required to get the managed hosts to the desired state.

In simple words, the howmany times we run the Ansible playbook, it might yeild the same results.  Unless we modify something in the playbook.

## what-is-ansible-tower
Ansible Tower was the enterprise web-based management platform for Ansible. It provided a graphical user interfInstead of running playbooks manually from the command line, we can run playbooks from UI. 
