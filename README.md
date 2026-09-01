# ai

Shared agent setup for my machines: coding standards, Codex delegation policy,
and skills, installable into both Claude Code and Codex as a plugin.

What lives where:

- `AGENTS.md` — tool-neutral coding standards. `scripts/sync` stamps it into
  `~/.codex/AGENTS.md` between its BEGIN/END markers; Claude reads the same
  file through an import in `~/.claude/CLAUDE.md` (sync adds it if missing).
  Machine-specific context (like ci4's hub-host block from dcvz/hub) lives in
  other markers in that file and is left alone.
- `CLAUDE.md` — Claude-only policy, injected each session by the plugin's
  SessionStart hook. Updates ride plugin auto-update, no sync involved.
- `skills/`, `agents/` — shared by both plugin manifests. Plugin skills are
  namespaced: `/ai:deslop`, `/ai:prune-comments`, `/ai:codex-delegate`.

## Install (once per machine)

```
claude plugin marketplace add git@github.com:dcvz/ai.git
claude plugin install ai@dcvz
codex plugin marketplace add git@github.com:dcvz/ai.git
codex plugin add ai@dcvz
git clone git@github.com:dcvz/ai.git ~/.cache/dcvz-ai && ~/.cache/dcvz-ai/scripts/sync
```

The ssh URL matters: the repo is private, so the `owner/repo` shorthand can't
fetch it.

## Updates

Claude auto-updates the plugin once the marketplace has auto-update on: either
toggle it in `/plugin` → Marketplaces → dcvz, or set `"autoUpdate": true` on
the `extraKnownMarketplaces.dcvz` entry in `~/.claude/settings.json`.

Codex has no auto-update, and the stamped `~/.codex/AGENTS.md` updates on
neither tool by itself, so schedule the sync — cron on Linux, and on macOS run
the crontab command from a real terminal so you can approve the one-time TCC
prompt (a headless agent hangs on it; a launchd agent works too):

```
( crontab -l 2>/dev/null; echo '17 9 * * * $HOME/.cache/dcvz-ai/scripts/sync >/dev/null 2>&1' ) | crontab -
```

Codex resolves plugin versions from `.codex-plugin/plugin.json` and caches per
version, so bump that version when skills change; sync's `codex plugin
marketplace upgrade` picks it up.
