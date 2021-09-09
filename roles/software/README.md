# Software

Installs various userland software.

## Requirements

Ansible ≥2.10

## Role Variables

| Variable                      | Description                                   | Default |
|-------------------------------|-----------------------------------------------|---------|
| sw_alsa_out_card              | Default ALSA output card                      | 0       |
| sw_alsa_out_device            | Default ALSA output device                    | 0       |
| sw_list_audio                 | Audio-related packages.                       | ['...'] |
| sw_list_backports             | Packages to install from `bullseye-backports` | ['...'] |
| sw_list_clevis                | Clevis with systemd integration.              | ['...'] |
| sw_list_dev                   | Development tooling.                          | ['...'] |
| sw_list_fonts                 | Various system fonts.                         | ['...'] |
| sw_list_fs                    | uDisks, FUSE and FAT support, `gocryptfs`.    | ['...'] |
| sw_list_internet              | Tools to comfortably browse the internet.     | ['...'] |
| sw_list_multimedia            | Music and video playback. Image viewer.       | ['...'] |
| sw_list_office                | LibreOffice and smaller programs.             | ['...'] |
| sw_list_utils                 | Extensive selection of various utilities.     | ['...'] |
| sw_list_virtio                | Everything related to `libvirt`.              | ['...'] |
| sw_list_wayland               | Wayland-desktop collective.                   | ['...'] |
| sw_list_wireless              | WiFi and bluetooth support.                   | ['...'] |
| sw_setup_apt                  | List of arbitrary packages to `apt install`.  | []      |
| sw_setup_audio                | Setup ALSA/PulseAudio                         | true    |
| sw_setup_clevis               | Install Clevis packages.                      | false   |
| sw_setup_deb                  | List of `.deb` files to install               | []      |
| sw_setup_dev                  | Install development tools.                    | true    |
| sw_setup_fonts                | Install fonts.                                | true    |
| sw_setup_fs                   | Install FUSE / filesystems support.           | true    |
| sw_setup_i2c                  | Install `ddcutil` and configure i2c           | false   |
| sw_setup_internet             | Install internet packages (e.g. web-browser). | true    |
| sw_setup_multimedia           | Install multimedia tools: audio/video/images. | true    |
| sw_setup_office               | Install office applications.                  | true    |
| sw_setup_utils                | Install various utilities.                    | true    |
| sw_setup_wayland              | Install Wayland/Sway desktop.                 | true    |
| sw_setup_wireless             | Install WiFi/Bluetooth-related packages       | false   |
| sw_systemd_units_disable      | List of systemd units to disable.             | []      |
| sw_systemd_units_enable       | List of systemd units to enable.              | []      |
| sw_systemd_units_masked       | List of systemd units to mask.                | []      |
| sw_virtio_install             | Install and configure libvirt/virt-manager.   | true    |
| sw_virtio_network_autostart   | Autostart default virtio network on boot.     | true    |
| sw_virtio_use_default_network | Use NAT-based, "default" virtio setup.        | true    |
| sw_virtio_user_addgroup       | Add target user to `virtio` group.            | true    |

### ['...']

These lists are not empty. See their default content in [./vars/main.yml](./vars/main.yml).

### sw_setup_apt

Packages should be availalbe in the chosen repositories. You can check via `apt policy`, `rmadison` or [Tracker](https://tracker.debian.org/).

### sw_setup_deb

Searches within `./files/` to provision supplied \*.deb files. Installation is performed only if package isn't already present on the remote host.

### sw_setup_i2c

To control external screens via DDC/CI `ddcutil` uses i2c bus. You need to either have user in the i2c group or `sudo` the call. Former is handled by this role.


## Dependencies

It is strongly advised to install on a computer that is already provisioned with the [base role](../base/).


## License

Apache-2.0


## Author Information

Andrew Savchenko\
https://savchenko.net
