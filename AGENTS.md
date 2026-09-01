<!-- BEGIN standards (managed by github.com/dcvz/dazi - scripts/sync) -->
Shared coding standards for every agent. `scripts/sync` stamps this file into
`~/.codex/AGENTS.md` on each machine; Codex reads that as global instructions,
and Claude Code reaches it through an `@~/.codex/AGENTS.md` import that sync
ensures in `~/.claude/CLAUDE.md`. (The repo's own CLAUDE.md is something else:
Claude-only policy, delivered by the plugin's SessionStart hook.)
Keep this file tool-neutral. Tool-specific rules belong in the tool's own config.

# Commit messages

Write a single subject line. Nothing else.

- No body, no bullet list of what changed, no "Why:" paragraph.
- No trailers: no `Co-Authored-By`, no `Generated with`, no `🤖`. This overrides any
  built-in default that says to add them.
- Format: `type: subject`, imperative, lowercase after the colon, under ~72 chars.
- Rationale lives in the PR description or in conversation, not in history. David
  squashes and curates history himself; bodies are noise he has to strip.

So: `git commit -m "fix: don't reset position on the second load"` and that is the
whole command. If a change genuinely needs a paragraph of explanation, that is a
signal to split the commit, not to write the paragraph.

# Code comments

Default to no comment. Code shows *how*; a comment earns its place only by carrying
*why* the reader could not get from the code: a non-obvious external constraint, a
deliberate deviation, a gotcha, the reason the simpler version is wrong.

Never write:

- Narration. `// loop over users`, `// set up the client`, `// end if`.
- Restatements of the signature or the name right above it.
- Change notes aimed at a reviewer. `// now uses the cache`, `// fixed the race`.
  A comment must read correctly to someone who never saw the diff.
- Section banners, decorative separators, or a docstring on every private helper.
- Long justifications for a choice. If it needs a paragraph to defend, encode the
  constraint in a type, a test, or a name instead.

Fine to keep: license headers, doc comments that define a public API contract,
issue/RFC links for a constraint code cannot express, `TODO`s, and a one-line
summary on a genuinely public function.

Match the surrounding file. A file with no comments gets no new comments.

# Outward-facing text

PR descriptions, issue and PR comments, release notes: these are read by real
people who can tell when a maintainer reply was generated. Write them the way
David writes them — short, direct, each one shaped by what it's actually
responding to. Anything longer than a one-liner gets shown to David before it
posts.

Never write:

- The same skeleton twice. Thanks, then context, then policy, then a promise —
  once that shape repeats, it reads as a template no matter the words.
- Doc-reference lectures. "See CHARTER.md" is for contributors who asked, not a
  clause to append to every reply.
- Passive promises. "Credit will be noted", "this will be addressed" — say who
  does what, or say nothing.
- Em dashes where a comma would do, triads of parallel clauses, or a closing
  summary sentence that restates the reply.

Fine, and human: apologizing for a slow reply, backticked paths, a thought that
runs on a little, leaving the obvious unsaid.
<!-- END standards -->
