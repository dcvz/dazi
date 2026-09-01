# dazi

Shared agent setup for my machines: coding standards, Codex delegation policy,
and skills, installable into both Claude Code and Codex as a plugin.

## Install

```
claude plugin marketplace add git@github.com:dcvz/dazi.git
claude plugin install dazi@dcvz
codex plugin marketplace add git@github.com:dcvz/dazi.git
codex plugin add dazi@dcvz
```

That's it. On Claude the plugin is self-contained: a SessionStart hook injects
the standards (`AGENTS.md`) and the Claude-only policy (`CLAUDE.md`) into every
session, and the skills come along namespaced (`/dazi:deslop`,
`/dazi:prune-comments`, `/dazi:codex-delegate`, `/dazi:standards`).

Codex plugins can only carry skills, not always-on instructions, so on Codex
the standards arrive as the `standards` skill, which triggers when committing,
commenting, or writing outward-facing text. For always-on standards on Codex,
add the sync below.

The ssh URL matters while the repo is private: the `owner/repo` shorthand
can't fetch it.

## Updates

Claude auto-updates the plugin once the marketplace has auto-update on: either
toggle it in `/plugin` → Marketplaces → dcvz, or set `"autoUpdate": true` on
the `extraKnownMarketplaces.dcvz` entry in `~/.claude/settings.json`.

Codex resolves plugin versions from `.codex-plugin/plugin.json` and caches per
version. CI bumps that version whenever the payload changes, and any
`codex plugin marketplace upgrade` (the sync runs one) picks it up.

## Optional: always-on standards for Codex (my fleet)

`scripts/sync` stamps `AGENTS.md` into `~/.codex/AGENTS.md` between BEGIN/END
markers — Codex then reads it as global instructions — and ensures
`~/.claude/CLAUDE.md` imports that file. Machine-specific context (like ci4's
hub-host block from dcvz/hub) lives between other markers in the same file and
is left alone. The plugin's hook detects the stamped block and skips its own
standards injection, so nothing is delivered twice.

```
git clone git@github.com:dcvz/dazi.git ~/.cache/dazi && ~/.cache/dazi/scripts/sync
```

Schedule it — cron on Linux; on macOS run the crontab command from a real
terminal so you can approve the one-time TCC prompt (a headless agent hangs on
it; a launchd agent works too):

```
( crontab -l 2>/dev/null; echo '17 9 * * * $HOME/.cache/dazi/scripts/sync >/dev/null 2>&1' ) | crontab -
```
