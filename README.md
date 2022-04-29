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

## The interesting variables

Most often you want your haproxy to do something other than just serving statistics. This is where the `destinations` variables come into play.

Currently this role only supports a very simple configuration, which is specified by using a type of `simple`. This role might get enhanced later to support more advanced use cases.

Here is an example:

```
listen postgres
    bind *:5000
    option httpchk
    http-check expect status 200
    default-server inter 3s fall 3 rise 2 on-marked-down shutdown-sessions
    server postgresql1 192.168.1.5:5432 maxconn 100 check port 8008
    server postgresql2 192.168.1.6:5432 maxconn 100 check port 8008
```

We have one `listen` section called `postgres`, that has two backend servers. They are serving traffic on port `5432`, while the liveness check is to be done using HTTP on port 8008.

The variables used to get this exact configuration look like this:

```
- role: 'install_and_configure_haproxy'
  vars:
    destinations:
      - type: 'simple'
        name: 'postgres'
        port: '5000'
        option: 'httpchk'
        option_configuration: 'expect status 200'
        default_server: 'inter 3s fall 3 rise 2 on-marked-down shutdown-sessions'
        backend_options: 'maxconn 100 check port 8008'
        backend_port: '5432'
        backends_group: 'postgresql_nodes'
```

As you can see, we have a list of destinations with only one element called `postgres`. This destination specifies the desired port, where haproxy should listen for connections (in this case port `5000`). It forwards the traffic to all the servers in the Ansible group `postgresql_nodes` as specified by the `backends_group` variable. In the example, the group `postgresql_nodes` has two members, `postgresql1` and `postgresql2`.

The port on the backends is specified in `backend_port`, the options used on all backends are specified in `backend_options`.

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: 'johanneskastl.install_and_configure_haproxy'
          vars:
            destinations:
              - type: 'simple'
                name: 'apache'
                port: '80'
                option: 'httpchk'
                option_configuration: 'expect status 200'
                default_server: 'inter 3s fall 3 rise 2 on-marked-down shutdown-sessions'
                backend_options: 'maxconn 100'
                backend_prefix: 'apache_'
                backend_port: '80'
                backends_group: 'webservers'

License
-------

BSD-3-Clause

Author Information
------------------

I am Johannes Kastl, reachable via kastl@b1-systems.de.
