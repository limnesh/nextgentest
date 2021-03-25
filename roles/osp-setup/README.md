osp-setup
=========

Element creation for the instances deployment in openstack (flavor, image, keypair, network and service-groups)

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
Network details for the creation:

osp_networks:
  # Public Facing Network   
  external:
    cloud_name: openstack
    network_name: ext_network
    provider_network_type: flat
    provider_physical_network: datacentre
    subnet_name: ext_subnet
    cidr_network: "192.0.2.0/24"
    state: present
    external: true
    gateway_ip: 192.0.2.254
    admin_state: yes
    ip_version: 4
    enable_dhcp: no
    allocation_pool_start: 192.0.2.150
    allocation_pool_end: 192.0.2.200


  # Private Facing Network   
  internal:
    cloud_name: openstack
    network_name: int_network
    subnet_name: int_subnet
    cidr_network: "172.16.0.0/24"
    state: present
    external: false
    admin_state: yes
    ip_version: 4
    enable_dhcp: yes

  # Public Facing Router connecting the two networks
osp_router:
  router:
    cloud_name: openstack
    state: present
    name: router
    network: ext_network
    interfaces: int_subnet

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

- hosts: workstation
  become: yes
  roles:
    - osp-setup


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
