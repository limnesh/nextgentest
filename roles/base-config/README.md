##Base Config

Configure server with base requisites

- Enable sudo
- Enable yum repository
- Install packages

##Requirements

None

##Role Variables

None

##Dependencies

None

##Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Setup base OS configuration
      hosts: all
      become: yes
      gather_facts: false
      roles:
        - {name: base-config, tags: base-config}i
