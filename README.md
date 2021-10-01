# Playbook

Configuration and maintenance of a host running Debian 11.


## Example usage

```sh
$ ansible-playbook playbook_local.yml --ask-become-pass --tags "role_base,role_software"
```


## Roles

### [base](/roles/base/)
Basic preparation of a target host.

### [software](/roles/software/)
Installation and configuration of various userland packages.

### [dotfiles](/roles/dotfiles/)
Distributes dotfiles to a target host.

### [transfer_agent](/roles/transfer_agent/)
Creates jailed user with a limited `chroot`. Supports automatic propagation of SSH keys.

### [wireguard](/roles/wireguard/)
Provisions Wireguard in any mode imaginable.

### [nginx](/roles/nginx/)
Configures web-server and enrolls an arbitrary number of domains on a remote `nginx` instance.

### [syncthing](/roles/syncthing)
Configures Syncthing client, relay and discovery servers.

### [etesync](/roles/etesync/)
EteBase server running on `uvicorn` and `nginx`, includes automatic HTTPs via `certbot`.

### [users](/roles/users/)
Handles users creation and removal as well as the SSH key management.

### [hosts](/roles/hosts)
Flexible management of `/etc/hosts`.

### [known_hosts](/roles/known_hosts)
Handy autoupdater of the `known_hosts` fingerprints.

### [ubuntu2debian](/roles/ubuntu2debian/)
Automatically converts a host running Ubuntu 20.04 into Debian 11.
