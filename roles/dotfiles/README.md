# Dotfiles

Provision dotfiles from this repository.

1. Backup what might be replaced.
1. Either:
    1. Copy to the target host under `~/.local/etc/dotfiles` and link to respective locations within `$HOME`.
    2. Link from this role directly. This requires executing against `localhost`.
1. Adapt `.bashrc`, `.tmux.conf` and `.gitconfig` if requested.
1. Setup fortunes and nVim with plugins.


## Requirements

Ansible ≥2.10


## Role Variables

| Variable                | Description                                                      | Default |
|-------------------------|------------------------------------------------------------------|---------|
| dots_bash_emacs_mode    | Switch bash to "regular" Emacs-like mode.                        | false   |
| dots_change_tmux_prefix | Changes `tmux` prefix from <kbd>C-Space</kbd> to <kbd>C-b</kbd>. | true    |
| dots_dotfiles_list      | What, where and how, details below.                              | {}      |
| dots_extract_dotfiles   | Copy dotfiles from this role to the target, details below.       | false   |
| dots_remote_gitconfig   | Use `.gitconfig` without `gpgsign` and author's information.     | true    |
| dots_repo               | Link to the dotfiles repository                                  | '...'   |
| dots_setup_fortunes     | Copy quotes for `fortunes` and generate its search DB.           | false   |
| dots_setup_neovim       | All-inclusive, 5-stars neovim setup.                             | false   |

### `dots_repo`

Git repository from where dotfiles will be downloaded and stored under the `./files/` of this role.
Defaults to [my dotfiles](https://github.com/savchenko/dotfiles), change with your own.

### `dots_dotfiles_list`

List of hashes. Searched under Ansible's default `./files/`.

```yaml
  - { src: 'Xresources'   , dest: '.Xdefaults'    , state: 'link' }
  - { src: 'aliases'      , dest: '.aliases.sh'   , state: 'link' }
  - { src: 'bash_profile' , dest: '.bash_profile' , state: 'link' }
  ...
```


## Dependencies

`unzip` and `bat`. Latter is due to the handler that attempts to rebuild its themes cache.


## License

Apache-2.0


## Author Information

Andrew Savchenko\
https://savchenko.net
