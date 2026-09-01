# ai

Shared agent setup for my machines: coding standards, Codex delegation policy,
and skills, installable into both Claude Code and Codex as a plugin.

What lives where:

- `AGENTS.md` — tool-neutral coding standards. `scripts/sync` stamps it into
  `~/.codex/AGENTS.md` between markers; Claude reads the same file through an
  import in `~/.claude/CLAUDE.md`. Machine-specific context (like ci4's
  hub-host block from dcvz/hub) lives in other markers in that file and is
  left alone.
- `CLAUDE.md` — Claude-only policy, injected each session by the plugin's
  SessionStart hook. Updates ride plugin auto-update, not sync.
- `skills/`, `agents/` — shared by both plugin manifests. Skill names are
  namespaced when installed via plugin: `/ai:deslop`, `/ai:prune-comments`.

## Install (once per machine)

```
claude plugin marketplace add dcvz/ai
claude plugin install ai@dcvz
codex plugin marketplace add dcvz/ai
codex plugin add ai@ai
git clone git@github.com:dcvz/ai.git ~/.cache/dcvz-ai && ~/.cache/dcvz-ai/scripts/sync
```

Then in Claude: `/plugin` → Marketplaces → dcvz → Enable auto-update. Claude
takes care of itself after that. Codex doesn't auto-update, and AGENTS.md is a
stamped file on both, so cron the sync:

```
( crontab -l 2>/dev/null; echo '17 9 * * * $HOME/.cache/dcvz-ai/scripts/sync >/dev/null 2>&1' ) | crontab -
```
