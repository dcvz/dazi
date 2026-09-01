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
commenting, or writing outward-facing text.

The ssh URL matters while the repo is private: the `owner/repo` shorthand
can't fetch it.

## Updates

Claude auto-updates the plugin once the marketplace has auto-update on: either
toggle it in `/plugin` → Marketplaces → dcvz, or set `"autoUpdate": true` on
the `extraKnownMarketplaces.dcvz` entry in `~/.claude/settings.json`.

Codex has no background updater; run this occasionally:

```
codex plugin marketplace upgrade
```

CI bumps the version in `.codex-plugin/plugin.json` whenever the payload
changes, so the upgrade always sees new content (Codex caches per version).

Machine-specific context (like ci4's hub-host block) is not this repo's
business — it lives in `~/.codex/AGENTS.md`, managed per machine (on hub hosts
by dcvz/hub's provisioning), reaching Claude through an `@~/.codex/AGENTS.md`
import in `~/.claude/CLAUDE.md`.
