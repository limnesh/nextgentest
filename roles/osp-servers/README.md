osp-servers
===========

Instance creation according the info in var osp_servers

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
Example of dict variable used in role

osp_servers:
  frontend:
    name: frontend
    state: present
    image: rhel-guest
    key_name: ansible_ssh
    flavor: m1.medium
    security_group: frontend
    meta:
      - { group: frontends, deployment_name: QA}
  app1:
    name: app1
    state: present
    image: rhel-guest
    key_name: ansible_ssh
    flavor: m1.medium
    security_group: apps
    meta:
      - { group: apps, deployment_name: QA}
  app2:
    name: app2
    state: present
    image: rhel-guest
    key_name: ansible_ssh
    flavor: m1.medium
    security_group: apps
    meta:
      - { group: apps, deployment_name: QA}
  db:
    name: db
    state: present
    image: rhel-guest
    key_name: ansible_ssh
    flavor: m1.medium
    security_group: db
    meta:
      - { group: appdbs, deployment_name: QA}

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

- hosts: localhost
  connection: local
  become: no
  gather_facts: true
  roles:
   - osp-servers

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
