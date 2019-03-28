# Ansible Role: unbound

An Ansible Role that installs and configures `unbound` on Debian and OpenBSD.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):
    
    unbound_global_config: [optional]
    
The default configuration of the main configuration file of the Unbound server is available in 
`unbound_default_global_config` in `vars/main.yml`.
It is possible to override or add new options using the `unbound_global_config` dictionary.
The variables in the dictionary must respect the name and the grammar of the real `unbound.conf` file.

Note that if the `unbound_global_config` is undefined, the main configuration file won't be updated.
    
    unbound_extra_config: {}

This dictionary allows to add extra configuration files in the `/etc/unbound/unbound.conf.d` directory.
They will be loaded if the `include` parameter exists in the `/etc/unbound.conf` file.
The variables in the dictionary must respect the name and the grammar of the real `unbound.conf` file.
    
    unbound_resolvconf_replace: False
    unbound_resolvconf_configuration: {}

These variables allow to configure the `/etc/resolv.conf` file.
See the options to add as a dictionary in `man resolv.conf`.

    unbound_daemon_enabled: True

This variable enables the daemon and startup or not.

For more information, see https://nlnetlabs.nl/projects/unbound/about/

## Dependencies

- t18s.fr_pkg

## Example Playbook

    - hosts: all
      roles:
        - role: t18s.fr_unbound
          unbound_global_config: {}      
          unbound_extra_config:
            consul:
                # Allow insecure queries to local resolvers
                server:
                    do-not-query-localhost: "no"
                    qname-minimisation: "no"
                    domain-insecure: consul
        
                # Add consul as a stub-zone
                stub-zone:
                    name: consul
                    stub-addr: "{{ consul_simulation_api_bind }}@8600"
        
                # Add forwarder
                forward-zone:
                    name: "."
                    forward-addr: "{{ my_nameservers }}"
                    forward-first: "yes"
        
          unbound_resolvconf_replace: True
          unbound_resolvconf_configuration:
            nameserver: "{{ ['127.0.0.1'] | union(my_nameservers) }}"
       

## Todo

Make it available for OpenBSD.

## License

```
Copyright (c) 2019 Tristan Weil <titou@lab.t18s.fr>

Permission to use, copy, modify, and distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
```
