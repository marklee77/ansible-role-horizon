horizon ansible role
====================

The purpose of this role is to deploy horizon onto Ubuntu. 

Role Variables
--------------

- openstack_identity_region: RegionOne

Example Playbook
-------------------------

    - hosts: all
      sudo: True
      roles:
        - horizon

License
-------

GPLv2

Author Information
------------------

http://stillwell.me
