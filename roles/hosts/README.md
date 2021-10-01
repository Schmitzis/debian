# hosts

Updates `/etc/hosts` with the content of Ansible inventory. Can be executed against a remote, localhost or both simultaneously.

## Requirements

- Ansible ≥2.10 on the controller

## Role Variables

| Variable                | Description                                    | Default |
|-------------------------|------------------------------------------------|---------|
| hosts_local_all         | Add whole inventory to the local `/etc/hosts`? | False   |
| hosts_local_groups      | Add hosts from specific groups.                | []      |
| hosts_local_rewrite     | Start with the "empty" hosts on localhost?     | False   |
| hosts_remote_all        | Add whole inventory to remote hosts file?      | False   |
| hosts_remote_groups     | Add hosts from specific groups.                | []      |
| hosts_remote_rewrite    | Start with the "empty" hosts on the remote?    | False   |
| hosts_remote_same_group | Add hosts from the same group?                 | False   |
| hosts_remote_self       | Add remote node to its own hosts file?         | False   |

### Notes

- `hosts_*_all` explicitly excludes localhost record to avoid possible overlaps. Declare `hosts_remote_self` as `True` if this is desirable.

- `hosts_*_rewrite` removes all valid entries except of `127.*` prior to managing the rest.

- `hosts_*_groups` must contain valid entries, execution will stop if supplied groups are not defined in the inventory.


## Example playbook

```yaml
---
- hosts: all
  strategy: linear
  tasks:
  - name: Execute `hosts` role
    import_role:
      name: hosts
    vars:
      hosts_local_all: True
      hosts_local_groups: []
      hosts_local_rewrite: False
      hosts_remote_all: False
      hosts_remote_groups: ['shared_1', 'shared_2']
      hosts_remote_rewrite: True
      hosts_remote_same_group: True
      hosts_remote_self: False
```

## Dependencies

None.

## License

Apache 2.0

## Author Information

Andrew Savchenko\
https://savchenko.net
