# postfix-relay

A simple Ansible role to setup Postfix with basic settings and maps. The role is not decide to cover all features, but to setup relay on servers.

## Requirements

No specific requirements, but the role only has been tested with Debian and Ubuntu.

## Role Variables

* `postfix_main` a dictionary of settings for the `main.cf`, default is `{}`,
  Some default settings are stored under `postfix_main_defaults` and merged.
* `postfix_maps` a dictionary of maps to create and update, key is the full path
  like `/etc/postfix/sasl_password` and the value an array of lines. By default
  `/etc/postfix/sasl_password` is created empty.

See [defaults](defaults/main.yml) and the example below.

## Example Playbook

```yaml
- hosts: localhost
  become: true
  roles:
    - lazyfrosch.postfix-relay
  vars:
    postfix_main:
      relayhost: '[mail.example.com]:submission'
      smtp_connection_cache_on_demand: false
      smtp_tls_policy_maps: hash:/etc/postfix/tls_policy
      sender_canonical_maps: hash:/etc/postfix/sender_canonical_map

    postfix_maps:
      /etc/postfix/sasl_password:
        - mail.example.com user:password

      /etc/postfix/sender_canonical_map:
        - '@{{ ansible_fqdn }} server@example.com'
        - 'user@{{ ansible_fqdn }} user@example.com'

      /etc/postfix/tls_policy:
        - mail.example.com encrypt
        - '[mail.example.com]:587 encrypt'
```

## License

Copyright (C) 2021 [Markus Frosch](mailto:markus@lazyfrosch.de)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see [gnu.org/licenses](https://www.gnu.org/licenses/).
