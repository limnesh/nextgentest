App Tier

Ansible Role to:

- Install Tomcat and prerequisites
- deploy Tomcat application

Requirements
------------

None

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------

Example usage:


    - name: setup app tier
      hosts: apps
      become: yes
      gather_facts: false
      roles:
        - {name: app-tier, tags: [apps, tomcat]}
