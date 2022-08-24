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
```
hockeypuck_home: /var/lib/hockeypuck
hockeypuck_directory: "{{ hockeypuck_home }}/hockeypuck/"
hockeypuck_hostname: "{{ hostname }}"
hockeypuck_templates: "{{ hockeypuck_directory }}/contrib/templates"
hockeypuck_index_template: "{{ hockeypuck_templates }}/index.html.tmpl"
hockeypuck_vindex_template: "{{ hockeypuck_templates }}/index.html.tmpl"
hockeypuck_stats_template: "{{ hockeypuck_templates }}/stats.html.tmpl"
hockeypuck_webroot: "{{ hockeypuck_directory }}/contrib/webroot/"
hockeypuck_logfile: /var/log/hockeypuck/hockeypuck.log
hockeypuck_loglevel: INFO
hockeypuck_recondb: "{{ hockeypuck_home }}/recon.db"
hockeypuck_log_request_details: "true"
```

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
