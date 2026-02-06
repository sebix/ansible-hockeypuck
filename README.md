Hockeypuck
==========

This role installs [hockeypuck](github.com/hockeypuck/hockeypuck) with the available golang version.

Hockeypuck is a modern OpenPGP key server, written in go.

This role installs PostgreSQL and configures it as backend for hockeypuck.
hockeypuck is cloned to the server and built there.
systemd is used for managing the server.

Requirements
------------


Role Variables
--------------

List of variables with their defaults:
```yaml
hockeypuck_home: /var/lib/hockeypuck
hockeypuck_directory: "{{ hockeypuck_home }}/hockeypuck/"
hockeypuck_hostname: "{{ hostname|d(ansible_hostname)|d(inventory_hostname) }}"
hockeypuck_templates: "{{ hockeypuck_directory }}/contrib/templates"
hockeypuck_index_template: "{{ hockeypuck_templates }}/index.html.tmpl"
hockeypuck_vindex_template: "{{ hockeypuck_templates }}/index.html.tmpl"
hockeypuck_stats_template: "{{ hockeypuck_templates }}/stats.html.tmpl"
hockeypuck_webroot: "{{ hockeypuck_directory }}/contrib/webroot/"
hockeypuck_logfile: /var/log/hockeypuck/hockeypuck.log
hockeypuck_loglevel: INFO
hockeypuck_recondb: "{{ hockeypuck_home }}/recon.db"
hockeypuck_log_request_details: "true"
hockeypuck_peers: []
hockeypuck_blacklist: []
hockeypuck_conflux_recon_allowCIDRs: []
hockeypuck_conflux_recon_seenCacheSize: 72000
hockeypuck_hkp_bind: :11371
hockeypuck_git_version: branch-2.1
hockeypuck_contact: # no default
hockeypuck_hkp_ssl_enabled: false
hockeypuck_hkp_ssl_certificate: # no default
hockeypuck_hkp_ssl_key: # no default
hockeypuck_service_restart: false
```

### Hostname
The hostname is used to name the host of the hockeypuck instance in the configuration file.

If the user-defined variable `hockeypuck_hostname` is not defined,
- the user-defined variable `hostname` is used. If that is not defined,
- `ansible_hostname` is used, which requires [gathered facts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/gather_facts_module.html). If facts are not gathered,
- `inventory_hostname` is used as fallback, which is always defined.

### HKPS/SSL/TLS

To enable HKPS, set the enable parameter and give the paths to certificate and private key as follows:
```
hockeypuck_hkp_ssl_enabled: true
hockeypuck_hkp_ssl_certificate: "/path/to/cerificate"
hockeypuck_hkp_ssl_key: "/path/to/private/key"
```

### Peers

`hockeypuck_peers` can either be a simple list:
```yaml
- example.com
- example.net
```
which assumes the same address for http and recon, as well as the default ports 11370 and 11371. The peer name will be `peer$index`.

Or, `hockeypuck_peers` can be a mapping:
```yaml
- name: full example
  http_addr: example.net
  http_addr_port: 11371
  recon_addr: example.net
  recon_addr: 11370
```
The ports are optional.

If the hockeypuck server is behind a reverse proxy, set `hockeypuck_conflux_recon_allowCIDRs` to the IP address of the proxy. Hockeypuck only allows incoming recon from configured peers and this list of allowed network ranges.
```yaml
hockeypuck_conflux_recon_allowCIDRs:
  - 10.0.0.1/8
  - 192.168.0.1/32
```

### Key Blacklisting

Keys can be [blacklisted](https://github.com/hockeypuck/hockeypuck/issues/79#issuecomment-735447403) with the `hockeypuck_blacklist` option with one fingerprint per list item:

```yaml
hockeypuck_blacklist:
  - B4530375102C9EB270909C9C006694EB
  - a490d0f4d311a4153e2bb7cadbb802b258acd84f
```

For an existing useful blacklist, have a look at [CIRCL's openpgp-keys-filterlists](https://github.com/CIRCL/openpgp-keys-filterlists).

### Service

`hockeypuck_service_restart` can be set to true to always restart the service with systemd if the hockeypuck process exits

Dependencies
------------

Installs go on the server with [`gantsign.golang`](https://galaxy.ansible.com/gantsign/golang).

Example Playbook
----------------

    - hosts: servers
      roles:
        - sebix.hockeypuck
      vars:
         # optional variables, see above for a complete list, including default values
        - golang_version: 1.19
        - hockeypuck_contact: "YOUR_KEY_FINGERPRINT"

License
-------

BSD-3-Clause

Author Information
------------------

https://intevation.de/
