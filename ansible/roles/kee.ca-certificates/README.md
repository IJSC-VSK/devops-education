CA certificates
===============

Add additional CA certificates to the trusted certificates list for RHEL/CentOS, Debian/Ubuntu systems.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    ca_certificates: []

List of certificates that will be added to the target system.

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
         - kee.ca-certificates

*Inside `vars/main.yml`*:

    ca_certificates:
      - cavsk.crt

License
-------

BSD
