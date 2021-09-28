# ubuntu2debian

Automatically converts a host running Ubuntu 20.04 into Debian 11.\
Does all the conflict resolution, cleanup and package management.

Do note that all Canonical-specific stuff will be removed, this includes telemetry, "snaps" and so on.

## Requirements

- Target should be running Ubuntu 20.04
- Playbook must be executed with the linear strategy
- Ansible ≥2.10 on the controller
- [community.general collection](https://github.com/ansible-collections/community.general) ≥3.5.0

## Role Variables

| Variable           | Description            | Default                      |
|--------------------|------------------------|------------------------------|
| u2d_remove_users   | Users to remove        | ['snap_daemon', 'pollinate'] |
| u2d_allow_root_ssh | Allow `root` to login? | False                        |

## Example playbook

```yaml
---
- hosts: ubuntu_host
  strategy: linear
  tasks:
  - name: Execute `ubuntu2debian` role
    import_role:
      name: ubuntu2debian
    vars:
      u2d_remove_users: ['snap_daemon', 'gnats', 'pollinate']
      u2d_allow_root_ssh: True
```

## Dependencies

None.

## License

Apache 2.0

## Author Information

Andrew Savchenko\
https://savchenko.net
