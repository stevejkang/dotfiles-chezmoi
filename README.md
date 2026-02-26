# dotfiles-chezmoi

Managed by [chezmoi](https://www.chezmoi.io/).

## Setup (new machine)

```bash
# Install chezmoi and apply dotfiles
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply stevejkang/dotfiles-chezmoi
```

## Daily usage

```bash
# Add a file to chezmoi
chezmoi add ~/.config/tmux/tmux.conf.local

# Edit a managed file (opens in $EDITOR, auto-applies on save)
chezmoi edit ~/.config/tmux/tmux.conf.local

# Preview changes before applying
chezmoi diff

# Apply changes from source to home
chezmoi apply

# Pull latest from remote and apply
chezmoi update
```

## Secrets (1Password)

Requires [1Password CLI](https://developer.1password.com/docs/cli/). Configure in `~/.config/chezmoi/chezmoi.toml`:

```toml
[onepassword]
  command = "op"
```

### Template: inject secret values into config files

When a config file contains partial secrets. Only placeholders are stored in the repo; actual values are fetched from 1Password at `chezmoi apply` time.

```bash
chezmoi add --template ~/.config/some-app/config.toml
```

```toml
# source: dot_config/some-app/config.toml.tmpl
[credential]
  token = "{{ onepasswordRead "op://Vault/Item/token" }}"
```

### Encrypt: store entire files encrypted

When an entire file is secret (e.g. SSH private keys, credentials.json). The file is stored age-encrypted in the repo and decrypted at `chezmoi apply` time using an age key stored in 1Password.

```bash
# 1. Generate age key and store in 1Password
age-keygen | op document create --title "chezmoi age key" --vault Personal -

# 2. Add age config to chezmoi.toml
#    encryption = "age"
#    [age]
#      identity = "{{ onepasswordDocument "chezmoi age key" "Personal" }}"
#      recipient = "age1..."  # public key from age-keygen output

# 3. Add a file as encrypted
chezmoi add --encrypt ~/.ssh/id_ed25519
# Stored as encrypted_private_dot_ssh/private_id_ed25519.age
```

## Git workflow

```bash
chezmoi cd                                      # cd into source directory
git add -A && git commit -m "..." && git push   # commit and push
exit                                            # back to previous directory
```

## What's managed

| File | Description | Prerequisite |
|---|---|---|
| `~/.config/tmux/tmux.conf.local` | tmux config (oh-my-tmux + powerkit) | `tmux` `bash` ≥5.0 (brew), [gpakosz/.tmux](https://github.com/gpakosz/.tmux) (git clone), Nerd Font (cask) |
| `~/.gitignore-global` | Global gitignore | `git` (system-installed) |
| `~/.gitconfig` | Git configuration (GPG signing enabled) | `git` (system-installed), `gnupg` (brew) |
| `~/.ssh/config` | SSH client config | `ssh` (system-installed) |
| `~/.ssh/id_ed25519` | SSH private key (age-encrypted) | `ssh` (system-installed) |
| `~/.ssh/id_ed25519.pub` | SSH public key | `ssh` (system-installed) |
| `~/.hushlogin` | Suppress login message | system-installed |
| `~/.gnupg/common.conf` | GnuPG common config | `gnupg` (brew) |
| `~/.gnupg/gpg-agent.conf` | GPG agent config (pinentry) | `gnupg` `pinentry-mac` (brew) |
| `~/.gnupg/public-key.asc` | GPG public key (armored export) | `gnupg` (brew) |
| `~/.gnupg/secret-key.asc` | GPG secret key (age-encrypted) | `gnupg` (brew) |
| `~/.gnupg/trustdb.gpg` | GPG trust database | `gnupg` (brew) |
| `~/.config/karabiner/karabiner.json` | Karabiner-Elements key remapping | [Karabiner-Elements](https://karabiner-elements.pqrs.org/) (cask) |
