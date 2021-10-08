# Syncthing

Configures [Minisync](https://github.com/savchenko/minisync) on the target host. Likely to work with the upstream Syncthing just as well.


## Requirements

- Ansible ≥2.10
- [community.general collection](https://github.com/ansible-collections/community.general) ≥3.5.0
- Python ≥3.9


## Role Variables

| Variable          | Description                          | Default               |
|-------------------|--------------------------------------|-----------------------|
| st_certs_path     | Certificates store                   | '../files/certs/host' |
| st_client         | Hash of client's parameters.         | {}                    |
| st_disco          | Hash of discovery server parameters. | {}                    |
| st_install_client | Install client?                      | False                 |
| st_install_disco  | Install discovery server?            | False                 |
| st_install_relay  | Install relay server?                | False                 |
| st_reinstall      | Re-write existing setup?             | False                 |
| st_relay          | Hash of relay server parameters.     | {}                    |

### Certificates storage

Role allows Syncthing certificate keypais to be:

- Downloaded from the remote to localhost for future use
- Uploaded to the remote so that fingerprints don't change
- Re-generated on the remote without involvement of localhost

`st_certs_path` is set to `{{ role_path }}/files/certs/{{ inventory_hostname }}` by default.

### Parameter structs

#### Syncthing client

```yaml
st_client: {
  'addr': '' ,                  # Address on which Client will listen, default: 127.0.0.1
  'addr_use_wan': true,         # Overwrite `st_client.addr` with the discovered WAN ip?
  'announce_broadcast': true,   # Enable "local discovery"?
  'certs_import': false,         # Save existing certificates?
  'certs_recreate': true,       # Re-create certificates? Yields new ID.
  'config_dir': '',             # Will be replaced with $XDG_CONFIG_HOME/syncthing if empty
  'gui': true,
  'gui_tls': true,
  'gui_user': '',               # GUI/API username
  'gui_user_password', '',      # GUI user password, will be hashed using BCrypt
  'min_free_space': 5,          # Minimum free disk space to keep
  'nat': false,                 # Uses upstream-provided STUN servers, list below.
  'port': '',                   # Port on which Client's GUI/API will listen, default: 8384
  'reconfigure': true,          # Overwrite existing config?
  'untrusted': true,            # Set device as "untrusted" by default?

  'user': {
    'name': '',                 # Defaults to SSH user
    'home': '',                 # Will be replaced with $HOME if empty
    },

  'default_folder': {
    'path': '~/sync/default',   # Will be created if does not exist
    'type': 'sendreceive',      # `receiveencrypted` can't be set on the *default* folder
    'pause': True               # Pause default folder?
    },

  'disco': {
    'addr': '127.0.0.1',        # Discovery-server to which Client will connect
    'addr_overwrite': true,     # Overwrite Disco address with the server from this role?
    'port': 8443,               # Port on which Client will try to find Disco, default: 8443
    'id': '',                   # ID of Disco to which Client will connect
    'id_overwrite': true,       # Overwrite `st_client.disco.id` with the `st_disco.id`?
    'enable': true              # Use "global announcements" aka Discovery-server?
    },

  'relay': {
    'enable': true,             # Enable relaying?
    'addr': '',
    'addr_overwrite': true,     # Overwrite Relay address with the server from this role?
    'port': 22067,
    'id': '',
    'id_overwrite': true,       # Overwrite `st_client.relay.id` with the `st_relay.id`?
    'reconnect_interval_minutes': 10
    },

  'start': {                    # Configure autostart via systemd
    'on_boot': true,
    'on_role_completion': 'restarted'   # Either: 'reloaded', 'restarted' or 'stopped'
  },

  'binary': {
    'shasum': '03ba1fb7326d0893fc5664602694144bbdf7717a895fb554fe89fb996f116433',
    'url': 'https://github.com/savchenko/minisync/releases/download/v1.16.2/syncthing',
    }
}
```

#### Discovery server

```yaml
st_disco: {
  'addr': '127.0.0.1',         # Address on which Disco listens itself
  'addr_use_wan': true,        # Use discovered WAN IP instead of the provided address?
  'certs_import': false,        # Save existing certificates?
  'certs_recreate': true,      # Re-create certificates? Yields new ID.
  'id': '',                    # Disco's own ID, *will* be overwritten during run
  'port': '',                  # Port on which Disco will listen itself, default: 8443
  'replicate': false,          # Add `st_disco_replica` into the systemd config?

  'replica': {
    'addr': '10.0.0.1',        # Address which Disco will use for replication
    'port': 19200,             # Port which Disco will use for replication
    'id': '',                  # ID of another replica instance
    'listen_addr': '10.0.0.2', # Address for incoming replication connections
    'listen_port': 19200,      # Port for incoming replication connections, default: 19200
    },

  'start': {                   # Configure autostart via systemd
    'on_boot': true,
    'on_role_completion': 'restarted'   # Either: 'reloaded', 'restarted' or 'stopped'
  },

  'binary': {
    'shasum': '6988838343dbf98eacb6678d551ae298fa85c1c0a4b0e2b2cff62e7228c442aa',
    'url': 'https://github.com/savchenko/minisync/releases/download/v1.16.2/stdiscosrv',
    }
}
```


#### Relay server

```yaml
st_relay: {
  'addr': '',
  'addr_use_wan': true,
  'certs_export': false,       # Upload existing certificates?
  'certs_import': false,         # Save existing certificates?
  'certs_recreate': true,
  'network_timeout_string': '2m0s',
  'ping_interval_string': '1m0s',
  'port': 22067,

  'nat': {
    'enabled': false,          # Use UPnP/NAT-PMP to acquire external port mapping?
    'lease_minutes': 60,
    'renewal_minutes': 30,
    'timeout_seconds': 10
  },

  'start': {                   # Configure autostart via systemd
    'on_boot': true,
    'on_role_completion': 'restarted'   # Either: 'reloaded', 'restarted' or 'stopped'
  },

  'binary': {
    'shasum': '715525cb0e835153a1ff465f312009c2ffc4d5d28294d850018087ee5727d238',
    'url': 'https://github.com/savchenko/minisync/releases/download/v1.16.2/strelaysrv'
  }
}
```


### Notes

- If you are running an instance of Syncthing on the discovery server, you must either add that instance to other devices using a static address or bind the discovery server and Syncthing instances to different IP addresses.

- Role executes against user that is connected to the target host (`ansible_env.USER`). Ensure that user can `sudo` and provide password with `--ask-become-pass` if necessary.

- `st_client_disco_id` and `st_client_disco_addr` must be defined as pair. If _both_ are undefined, then Syncthing will use the default ones.

- Automatic upgrades and telemetry are disabled.

- Firewall setup is not covered in this role.

- To guess the "default" ipv4 of the target host, playbook is using this [this hack](https://medium.com/opsops/ansible-default-ipv4-is-not-what-you-think):
  ```yaml
  ansible_default_ipv4.address|default(ansible_all_ipv4_addresses[0])
  ``````

- There is a hard-coded list of STUN servers which are contacted if `st_client.nat` is enabled.

    ```
    stun.syncthing.net:3478
    stun.callwithus.com:3478
    stun.counterpath.com:3478
    stun.counterpath.net:3478
    stun.ekiga.net:3478
    stun.ideasip.com:3478
    stun.internetcalls.com:3478
    stun.schlund.de:3478
    stun.sipgate.net:10000
    stun.sipgate.net:3478
    stun.voip.aebc.com:3478
    stun.voiparound.com:3478
    stun.voipbuster.com:3478
    stun.voipstunt.com:3478
    stun.xten.com:3478
    ```

- If Discovery-server replication is configured, it is assumed that bi-directional exchange of information is desirable. Keep in mind that is is possible to configure `stdiscosrv` to act [exclusively](https://docs.syncthing.net/branch/untrusted/html/users/stdiscosrv.html) as the listener or sender.

- "If you are running an instance of Syncthing on the discovery server, you must either add that instance to other devices using a static address or bind the discovery server and Syncthing instances to different IP addresses".

- Using 100::/64 (ipv6 null-route) for the `localAnnounceMCAddr` reduces the counter of "successful discoveries". Empty value does not have this behaviour, but shows an annoying notice in the web UI. Former option is set by default.

## Dependencies

Tested on a remote that is already provisioned with the [base role](../base/).


## License

Apache 2.0


## Author Information

Andrew Savchenko\
https://savchenko.net
