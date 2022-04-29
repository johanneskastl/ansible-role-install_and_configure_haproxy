![Ansible Lint](https://github.com/johanneskastl/ansible-role-install_and_configure_haproxy/workflows/Ansible%20Lint/badge.svg)

install_and_configure_haproxy
=========

Install and configure haproxy

Requirements
------------

None.

Role Variables
--------------

## General settings

These settings should be identical on most Linux distributions:

- `systemd_unit_name`: name of the systemd service (default: `haproxy.service`)
- `package_name`: name of the package for haproxy (default: `haproxy`)
- `config_file_path`: path to the haproxy configuration file (default: `/etc/haproxy/haproxy.cfg`)

## haproxy settings

- `publish_stats`: Whether statistics should be published (default: `true`)

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: 'johanneskastl.install_and_configure_haproxy'

License
-------

BSD-3-Clause

Author Information
------------------

I am Johannes Kastl, reachable via kastl@b1-systems.de.
