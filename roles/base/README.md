# Base

Basic setup of a sensible Debian host.\
Non-exhaustive summary, majority of these are optional:

- Run preflights checks to assert the OS, hardware and user's choices.
- Optionally upgrade Buster installation to Bullseye (v10 to v11).
- Setup hostname, locale and timezone
- Enforce kernel module signature verification if SecureBoot is configured.
- Configure GRUB and `GRUB_CMDLINE_LINUX_DEFAULT`, enable vendor-specific IOMMU.
- Setup `dpkg` overrides, `apt` and harden permissions of various paths.
- Install minimum-necessary set of packages, enable `unattended-upgrades`.
- Configure `knot-resolver` as the default DNS resolver and `timesyncd` as the NTP client.
- Generate unique set of moduli for DH key exchange, configure `sshd`.
- Configure various sysctl tunables and `systemd` settings.
- Disable bluetooth, thunderbolt & firewire as well as some other kernel modules.
- Configure Caps Lock key as Control modifier, setup framebuffer locale and font.
- Reduce preservation times for various logs across the system.
- If explicitly allowed, automatically reboot target host at the end.

Have a look at variables section below.

## Requirements

Ansible ≥2.10

## Role Variables

| Variable                       | Description                                                              | Default            |
|--------------------------------|--------------------------------------------------------------------------|--------------------|
| base_allow_reboot              | Automatically reboot target machine if necessary.                        | False              |
| base_autoupgrade               | Automatically upgrade from Buster to Bullsye?                            | False              |
| base_consoleblank              | Turn-off screen when staring at blank TTY after X seconds.               | 600                |
| base_debsecan                  | Install and configure `debsecan`?                                        | True               |
| base_disable_bluetooth         | Likewise.                                                                | True               |
| base_disable_bt_autosuspend    | Disable Bluetooth autosuspend. Useful with buggy handsfree devices.      | False              |
| base_disable_firewire          | Similar to the above.                                                    | True               |
| base_disable_hfs               | ...                                                                      | True               |
| base_disable_nonsof            | Blacklist `snd_hda_intel` and `snd_soc_skl` kernel modules.              | False              |
| base_disable_speaker           | Internal "beeper" only, nothing to do with ALSA.                         | True               |
| base_disable_thunderbolt       | Blacklist Thunderbolt kernel modules. [See why](https://thunderspy.io/). | True               |
| base_enable_audit              | Enable kernel audit framework.                                           | False              |
| base_fb_configure              | Configure framebuffer/console: font size, encoding, etc.                 | True               |
| base_fb_font_size              | Framebuffer font size                                                    | 10x18              |
| base_fstab_noexec              | Mount /dev/shm with nodev, nosuid, and noexec.                           | True               |
| base_fstab_noexec_tmp          | Mount /tmp with noexec.                                                  | False              |
| base_fw_list                   | List of firmware files to download and install on target.                | []                 |
| base_generate_moduli           | Generate new set of 4096 DH moduli.                                      | False              |
| base_grub_hardened             | "Harden" the GRUB config?                                                | True               |
| base_grub_optional             | Additional options for `GRUB_CMDLINE_LINUX_DEFAULT`.                     | -                  |
| base_grub_timeout              | Timeout of the default GRUB menu in seconds.                             | 1                  |
| base_hda_options               | List of options that will be applied to `snd-hda-intel`.                 | []                 |
| base_hide_pid                  | Mount /proc with `hidepid=2`. Not advised on desktops.                   | false              |
| base_init_on_free              | wipe-on-free, see msg. `20190626121943.131390` for the explanation.      | False              |
| base_install_recommends        | Install recommended packages by default?                                 | True               |
| base_kresd_blocklist           | Optional path to the blocklist _in RPZ format_.                          | ""                 |
| base_kresd_install             | Install Knot DNS resolver. Recommended to enable.                        | False              |
| base_kresd_listen_on_ip        | Address on which Knot will listen.                                       | 127.0.0.1          |
| base_kresd_primary_hostname    | ...                                                                      | dns.quad9.net      |
| base_kresd_primary_ip          | Self-explanatory                                                         | 9.9.9.9            |
| base_kresd_secondary_hostname  | ...                                                                      | dns.quad9.net      |
| base_kresd_secondary_ip        | ...                                                                      | 149.112.112.112    |
| base_kresd_tls                 | Boolean, forward queries via TCP/TLS or UDP                              | True (TLS)         |
| base_locale                    | For example, "en_GB.UTF-8".                                              | en_AU.UTF8         |
| base_locale_singular           | Instruct `apt` to avoid installing extra languages.                      | True               |
| base_logind_configure          | Handle idle via `logind`? Configures `/etc/logind.conf`.                 | false              |
| base_logind_idle_action        | What to do when machine is idle.                                         | "suspend"          |
| base_logind_idle_time          | Idle time in seconds.                                                    | 600                |
| base_logind_lid_action         | What to do when the lid is closed.                                       | "suspend"          |
| base_logind_powerbutton_action | Action to execute when power button is pressed.                          | "poweroff"         |
| base_nmi_watchdog              | Enable NMI watchdog?                                                     | True               |
| base_pam_namespace             | Enable `pam_namespace.so`?                                               | False              |
| base_passwd_timeout            | `sudo` **password prompt** timeout, in minutes. Default is 12s.          | '0.2'              |
| base_pkg_remove                | List of packages to remove.                                              | ['rpcbind']        |
| base_reports_email             | E-mail address for system reports                                        | 'root'             |
| base_require_tty_pty           | Set `use_pty` & `requiretty` in `/etc/sudoers`.                          | False              |
| base_set_capslock              | Set <kbd>CapsLock</kbd> as <kbd>Ctrl</kbd>.                              | False              |
| base_set_dpkg_overrides        | Tighten various filesystem permissions. Use with care!                   | False              |
| base_set_hostname              | Set target's hostname to the `inventory_hostname`                        | False              |
| base_set_sigenforce            | Enforce kernel modules signature verification, only with SecureBoot.     | True               |
| base_sleep_enabled             | Enable (hybrid) suspend and sleep?                                       | False              |
| base_sshd_accelerate           | Exclusively use AES128GCM and UMAC128ETM.                                | False              |
| base_sshd_less_secure          | Enable aes256-cbc cipher and hmac-sha-256 MAC.                           | False              |
| base_sudo_cmd_nopwd            | Comma-separated paths user can run with passwordless `sudo`.             | ""                 |
| base_sudo_set                  | Configure `sudo`-capable user?                                           | False              |
| base_sudo_timeout              | `sudo` timeout in minutes.                                               | 15                 |
| base_sudo_user                 | Allow this user to run `sudo`, defaults to SSH login unless it is root.  | `ansible_env.USER` |
| base_swappiness                | Start swapping when there is % memory left.                              | 30                 |
| base_thermald_profiles         | .XML profiles generated by `dptfxtract`                                  | []                 |
| base_timezone                  | Self-explanatory.                                                        | UTC                |
| base_trackpoint_drift          | Sets trackpoint drift time to a certain value. `0` disables the task.    | 25                 |
| base_vm_writeback              | Delay writes to disk by X ms. Debian's default is 500.                   | 1500               |

### base_fw_list

Example:

```yaml
base_fw_list:
  - { src: 'https://example.com/fw.bin',
      dest: 'misc/example_fw.bin', # path is relative to /lib/firmware
      checksum: 'sha256:696283c5fc5fef98f3269f32aaf446fed20029ae6c5eee23684a3030dde1400d' }
```


## Role packages

### Default set

| Name                | Reason                                            |
|---------------------|---------------------------------------------------|
| acl                 | ACL utilities.                                    |
| bash-completion     | Self-explanatory.                                 |
| ca-certificates     | Extensive set of certificates from Mozilla store. |
| curl                | `curl`.                                           |
| dialog              | Part of the base, missing in some "cloud" images. |
| irqbalance          | `irqbalance`                                      |
| libpam-passwdqc     | PAM module that QCs all user-provided passwords.  |
| libpam-tmpdir       | Configures $TMPDIR and $TMP per each session.     |
| lua5.4              | Required by Knot and others.                      |
| needrestart         | Checks what needs to be restarted after upgrades. |
| net-tools           | `arp`, `netstat`, etc.                            |
| rng-tools5          | `rngd`                                            |
| rsync               | `rsync`                                           |
| sudo                | `sudo`                                            |
| systemd-timesyncd   | Proper NTP client.                                |
| ufw                 | Handy interface for nftables.                     |
| unattended-upgrades | Installs security updates automatically.          |

`irqbalance` is installed only if target is not a VM.

### Common firmware

| Name                  | Description                                              |
|-----------------------|----------------------------------------------------------|
| bluez-firmware        | Firmware for Bluetooth devices.                          |
| firmware-atheros      | Binary firmware for Qualcomm Atheros wireless cards.     |
| firmware-iwlwifi      | Binary firmware for Intel Wireless cards.                |
| firmware-linux-free   | Binary firmware for various drivers in the Linux kernel. |
| firmware-misc-nonfree | Binary firmware for various drivers in the Linux kernel. |
| firmware-realtek      | Binary firmware for Realtek wired/wifi/BT adapters.      |

`fwupd-amd64-signed` is installed only if target is a physical computer.

### Platform-specific firmware

Packages are selected based on the detected CPU.

| Name                           | Description                                                                  |
|--------------------------------|------------------------------------------------------------------------------|
| amd64-microcode                | Processor microcode firmware for AMD CPUs.                                   |
| firmware-amd-graphics          | Binary firmware for Qualcomm Atheros wireless cards.                         |
| mesa-va-drivers                | Binary firmware for Intel Wireless cards.                                    |
| firmware-intel-sound           | Binary firmware for various drivers in the Linux kernel.                     |
| intel-media-va-driver-non-free | [H/W video acceleration](https://wiki.debian.org/HardwareVideoAcceleration). |
| intel-microcode                | Binary firmware for Realtek wired/wifi/BT adapters.                          |
| iucode-tool                    | Tools to manage UEFI firmware updates (signed).                              |

## Miscellaneous

### `thermald`

Will be configured if:
  1. `base_thermald_profiles` is a list containing ≥1 files.
  1. Detected CPU is Intel.

### S4

Hibernation is disabled in kernel when system is booted in the "lockdown" mode. This is the case when SecureBoot is enabled.

There is a patchset that might allow functional and secure hibernation in future: https://lore.kernel.org/lkml/20210220013255.1083202-1-matthewgarrett@google.com/T/#u


## Dependencies

None


## License

Apache-2.0


## Author Information

Andrew Savchenko\
https://savchenko.net
