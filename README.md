# Ansible Role: unbound

An Ansible Role that installs and configures `unbound`.

[![Actions Status](https://github.com/tristan-weil/ansible-role-unbound/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-unbound/actions)

## Role Variables

Available variables are listed below, (see also `defaults/main.yml`).

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| unbound_global_config | {} | the default configuration of the main configuration file of the Unbound server is available in `_unbound_default_global_config` in `vars/main.yml`. It is possible to override or add new options using the `unbound_global_config` dictionary. The variables in the dictionary must respect the name and the grammar of the real `unbound.conf` file. Note that if the `unbound_global_config` is undefined, the main configuration file won't be updated. |
| unbound_extra_config | [] | this list allows to add extra configuration files in the `/etc/unbound/unbound.conf.d` directory. They will be loaded if the `include` parameter exists in the `/etc/unbound.conf` file. The variables in the dictionary must respect the name and the grammar of the real `unbound.conf` file. |
| unbound_daemon_enabled  | True | *True / False*: to enable this service by default |
| unbound_etc_hosts | []  | a list of <*unbound_etc_host*> entries | |
| unbound_etc_hosts_file_name | etc_hosts.conf | the name of the `unbound_etc_hosts` file in the `unbound` directory |

### <*unbound_etc_host*>

A *unbound_etc_host* represents an entry of one or more hostnames and an associated IP address.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| hostname      | one or more hostnames |
| address       | an IP address |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| state         | present | *present / absent*: the state of the entry in the `/etc/hosts` file |

For more information, see https://nlnetlabs.nl/projects/unbound/about/

## Example Playbook

    - hosts: 'all'
      roles:
        - role: 'ansible-role-unbound'
          unbound_extra_config:
            consul:
                # Allow insecure queries to local resolvers
                server:
                    do-not-query-localhost: 'no'
                    qname-minimisation: 'no'
                    domain-insecure: consul
        
                # Add consul as a stub-zone
                stub-zone:
                    name: 'consul'
                    stub-addr: "{{ consul_simulation_api_bind }}@8600"
        
                # Add forwarder
                forward-zone:
                    name: "."
                    forward-addr: "{{ my_nameservers }}"
                    forward-first: 'yes'     

## Todo

None.

## Dependencies

See [requirements_galaxy.yml](https://github.com/tristan-weil/ansible-role-unbound/blob/master/requirements_galaxy.yml)

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-unbound/blob/master/meta/main.yml)

## License

See [LICENSE.md](https://github.com/tristan-weil/ansible-role-unbound/blob/master/LICENSE.md)
