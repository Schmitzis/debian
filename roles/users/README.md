# users

Add or remove users to a target host. Written after experiencing barrage of issues with the "simple" `users` and `authorized_keys` modules.

## Requirements

- Ansible ≥2.10 on the controller

## Role Variables

| Variable     | Description             | Default |
|--------------|-------------------------|---------|
| users_add    | List of users to create | []      |
| users_remove | List of users to remove | []      |

## Example playbook

```yaml
---
- hosts: all
  strategy: free
  tasks:
  - name: Execute `base` role
    import_role:
      name: users
    tags: role_users
    vars:
      users_remove: ['john', 'mary']
      users_add:
        - { name: 'ben',               # User is skipped if left empty
            groups: ['new_group'],     # Groups are created if necessary
            home: '/home/ben',         # Can be left empty
            home_chown: True,          # Make $HOME owned by the user?
            password: '',              # Use `'!'' to disable the account
            password_lock: False,      # `usermod -L` after creation?
            password_update: False     # Update password if user exists?
            shell: '/bin/bash',        # Defaults to "/usr/sbin/nologin"
            ssh_add_all: True,         # Add everything under ~/.ssh/*.pub?
            ssh_add_keys: [            # Add specific keys from ~/.ssh/
              'secret.k',
              'id_rsa.pub'] }
```

## Dependencies

None.

## License

Apache 2.0

## Author Information

Andrew Savchenko\
https://savchenko.net
