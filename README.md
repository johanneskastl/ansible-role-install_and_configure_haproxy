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

## OS-specific settings

These variables are loaded from OS-specific variable files, depending on the OS we are running on. The user should never need to change them.

- `chroot_path`: path to the chroot
- `stats_socket_path`: path to the stats socket
- `haproxy_user`: system user for haproxy
- `haproxy_group`: system group for haproxy

## haproxy settings

There are many many settings for haproxy. This role is based on the openSUSE haproxy configuration file, but it lets you finetune most of the settings.

Please have a look into the [defaults file ](defaults/main.yml), as there are too many to list here...

## publishing haproxy statistics

The user can decide if she wants to disable the statistics page. This can be done globally (for both IPv4 and IPv6) or for IPv6 only.

- `publish_stats`: Whether statistics should be published (default: `true`)
- `publish_stats_ipv6`: Whether statistics should be published over IPv6(default: `true`)

If the statistics should be published, the user can change the default port, URI and refresh interval:

- `stats_uri`: URI where statistics are published (default: `/`)
- `stats_port`: port where statistics are published (default: `80`)
- `stats_refresh_interval_seconds`: interval between refreshes (default: `5s`)

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
