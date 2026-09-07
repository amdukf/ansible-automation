# bind9

An Ansible role for managing BIND9 DNS records.

Supports adding, updating, and deleting DNS records such as `A`, `CNAME`, `NS`, and `TXT`.

## Requirements

- Ansible
- BIND9

## Role Variables

- `zone` — DNS zone name.
- `zone_file` — Path to the BIND9 zone file.
- `record_name` — DNS record name.
- `record_type` — DNS record type (`A`, `CNAME`, `NS`, `TXT`).
- `record_value` — DNS record value.

## Example Playbook

```yaml
- hosts: bind9
  become: true
  roles:
    - bind9