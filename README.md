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

Then two one-time steps:

1. Claude auto-update: `/plugin` → Marketplaces → dcvz → Enable auto-update
   (or set `"autoUpdate": true` on the `extraKnownMarketplaces.dcvz` entry in
   `~/.claude/settings.json`).
2. Nothing for Codex — its skills are picked up on install. (Older versions
   needed a "Hooks need review" approval; plugin hooks are gone as of CLI 0.153.)

On Claude the plugin is self-contained: a SessionStart hook injects
the standards (`AGENTS.md`) and the Claude-only policy (`CLAUDE.md`) into every
session, and the skills come along namespaced (`/dazi:writing`,
`/dazi:deslop`, `/dazi:prune-comments`, `/dazi:codex-delegate`,
`/dazi:standards`).

`AGENTS.md` mandates `writing` before any prose is drafted, so the deep
catalogue of AI tells loads on demand rather than sitting in context all
session.

On Codex the standards arrive through the `standards` skill, which fires when
committing, commenting, or writing prose. Codex removed plugin-provided hooks in
CLI 0.153 (`codex features list` shows `plugin_hooks` as `removed`), so there is
no always-on path there any more; the `hooks/` directory and the `hooks` key in
`.codex-plugin/plugin.json` are kept for Claude and in case Codex restores the
feature.

The ssh URL matters while the repo is private: the `owner/repo` shorthand
can't fetch it.

## Updates

Claude auto-updates on its own once the toggle above is set. Codex has no
background updater; run this occasionally:

```
codex plugin marketplace upgrade
```

CI bumps the version in `.codex-plugin/plugin.json` whenever the payload
changes, so the upgrade always sees new content (Codex caches per version).

Machine-specific context (like ci4's hub-host block) is not this repo's
business — it lives in `~/.codex/AGENTS.md`, managed per machine (on hub hosts
by dcvz/hub's provisioning), reaching Claude through an `@~/.codex/AGENTS.md`
import in `~/.claude/CLAUDE.md`.
