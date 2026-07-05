---
name: ssh-safe-remote
description: Safe, token-efficient remote SSH workflows for Codex. Use when running SSH commands, remote bash or Python, heredocs, rsync/scp-style transfers, or whenever zsh/bash quoting, wildcard expansion, nested quotes, python -c, or repeated SSH command boilerplate could cause errors.
---

# SSH Safe Remote

Use this skill for remote shell work. Prefer the bundled scripts over hand-built
`ssh 'python -c ...'` or deeply nested quote commands.

## Rules

- Run local shell commands with `/bin/bash` when composing SSH/heredoc commands.
- Use remote bash explicitly; do not rely on the remote login shell.
- Prefer single-quoted heredoc delimiters: `<<'REMOTE'` and `<<'PY'`.
- Put glob patterns inside the remote script, not in the local command string.
- Do not use `python -c` for multi-line remote Python. Use `sshpy`.
- Use `rscp` for transfers; it wraps rsync with compatible defaults.
- Keep output focused: print summaries and paths, not huge logs.

## Scripts

The installed skill directory is usually available inside the plugin under:

```bash
skills/ssh-safe-remote/scripts
```

Run remote bash:

```bash
scripts/sshb REMOTE_HOST <<'REMOTE'
set -euo pipefail
cd ~/project
rg -n "pattern" .
REMOTE
```

Run remote Python:

```bash
scripts/sshpy REMOTE_HOST <<'PY'
import glob
print(glob.glob("/tmp/results/*.json"))
PY
```

Transfer files:

```bash
scripts/rscp REMOTE_HOST:/tmp/results/ ./results/
```

## Patterns

Remote command with no heredoc:

```bash
scripts/sshb REMOTE_HOST 'cd ~/repo && git status -sb'
```

Remote loop without local glob expansion:

```bash
scripts/sshb REMOTE_HOST <<'REMOTE'
set -euo pipefail
for f in /tmp/run_*/summary.json; do
  [ -e "$f" ] || continue
  python3 -m json.tool "$f" | head -40
done
REMOTE
```

If authentication must be interactive, bypass these scripts and state why. The
scripts default to non-interactive SSH so Codex does not hang on password prompts.
