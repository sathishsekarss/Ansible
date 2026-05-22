Table of contents

1. [What is ansible ?](#what-is-ansible)
2. [How is ansible agentless ?](#ansible-is-agentless)
3. [How is ansible modules built?](#how-is-ansible-modules-built?)


## what-is-ansible
Ansible is an open-source automation tool used in linux for Configuration management, Application deployment, Provisioning, Orchestration.

## ansible-is-agentless
Ansible is agentless meaning we don't have install it on all the server ( child nodes - All linux os'es ) that we are goingto manage operations against.

It only needs to installed on the control node ( main node ) where we write the ansible playbook to control the child nodes.

## how-is-ansible-modules-built?
Ansible modules are mainly built using the python.  Most of the implementations we don't have to worry about.  It's ready to use.  We just have to use the modules in the tasks section or any other sections in the ansible playbooks.
