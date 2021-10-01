# known_hosts

The role does exactly two things:

1. Removes hosts from `$HOME/.ssh/known_hosts`
2. Updates corresponding SSH fingerprints

## Requirements

- Ansible ≥2.10 on the controller

## Role Variables

None.

## Example playbook

```yaml
---
- hosts: rehash_these_hosts
  strategy: free
  tasks:
  - name: Update known_hosts
    import_role:
      name: known_hosts
```

## Dependencies

None.

## License

Apache 2.0

## Author Information

Andrew Savchenko\
https://savchenko.net
