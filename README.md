# SSH Safe Remote for Codex

SSH Safe Remote is a small Codex plugin that packages a skill and helper
scripts for safer remote shell work.

It helps Codex avoid common SSH mistakes:

- local shell wildcard expansion before `ssh`,
- remote login shells such as zsh changing behavior,
- fragile nested quoting,
- multi-line `python -c` strings,
- repeated verbose SSH boilerplate.

## Install

In Codex CLI, add this repository as a plugin marketplace, then install the
plugin from that marketplace:

```bash
codex plugin marketplace add enkerewpo/codex-ssh-safe-remote --ref main
codex plugin add ssh-safe-remote --marketplace ssh-safe-remote-marketplace
```

Equivalent install selector:

```bash
codex plugin add ssh-safe-remote@ssh-safe-remote-marketplace
```

Start a new Codex thread after installation so the skill metadata is loaded.

## Contents

- `sshb`: run remote bash with consistent options.
- `sshpy`: run remote Python from a heredoc.
- `rscp`: copy files with rsync-compatible defaults.

## Privacy

This public plugin contains no hostnames, usernames, IP addresses, private
paths, keys, or project-specific configuration. Configure your own SSH hosts in
your local `~/.ssh/config`.
