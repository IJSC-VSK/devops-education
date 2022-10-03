# Ansible Role: HAProxy

Installs HAProxy on RedHat/CentOS.

**Note**: This role for HAProxy versions 2.x only. 

## Requirements

None

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    haproxy_socket: /var/lib/haproxy/stats

The socket through which HAProxy for administration or statistics. 
To disable directive, set haproxy_socket: ''

    haproxy_chroot: /var/lib/haproxy

The chroot() directory. 
To disable, set`haproxy_chroot: ''

    haproxy_user: haproxy
    haproxy_group: haproxy

A list of extra global vars for global configuration section (optional). 
Example:

    haproxy_global_vars: []
      - 'nbproc 1'
      - 'nbthread 2'
      - 'cpu-map auto:1/1-2 0-1'

## Dependencies

None

## Example Playbook

  hosts: all # host groups 
  become: true
  roles:
    - { role: kee.haproxy }

## License

BSD
