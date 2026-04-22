# slash-commands

Claude Code slash commands used across my workflow. Portable, versioned, machine-agnostic.

## What's here

| Command | Purpose |
|---|---|
| `advisory.md` | Advisory engagement methodology |
| `ai-governance.md` | AI governance framework invocation |
| `ai-roadmap.md` | Multi-year AI roadmap structure |
| `ai-strategy.md` | AI strategy synthesis |
| `export-pdf.md` | Export vault note → PDF via md-to-pdf |
| `poc-builder.md` | PoC Factory framework invocation |
| `sow-to-context.md` | Extract Client Context.md from SOW |

All commands use `$VAULT_ROOT` env var so they work on any machine where that's set.

## Install on a new machine

```bash
# 1. Clone
cd ~/AI-Projects
git clone https://github.com/hari3cloud/slash-commands.git

# 2. Symlink into the vault's .claude/commands/
VAULT_ROOT="${VAULT_ROOT:-$HOME/Library/CloudStorage/OneDrive-Personal/PersonalVault}"
mkdir -p "$VAULT_ROOT/.claude"
ln -sf "$HOME/AI-Projects/slash-commands/commands" "$VAULT_ROOT/.claude/commands"

# 3. Verify
ls "$VAULT_ROOT/.claude/commands/"
```

## Update workflow

Updates happen on Work Mac; other machines pull:

```bash
# On Work Mac after editing a command file:
cd ~/AI-Projects/slash-commands
# Copy latest from vault into repo
cp "$VAULT_ROOT/.claude/commands/"*.md commands/
git add -A
git commit -m "update: <which command> <what changed>"
git push

# On any other machine:
cd ~/AI-Projects/slash-commands && git pull
```

## Design notes

- Commands use `${VAULT_ROOT:-<Work Mac default>}` pattern — fall back to Work path if env var missing
- This repo is the source of truth; Work vault's `.claude/commands/` is effectively a working copy
- `.claude/` is excluded from OneDrive Work→Personal sync (to avoid leaking runtime state), so slash commands need this separate transport
