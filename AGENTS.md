Shared coding standards for every agent. Claude and Codex both get this file
always-on through the plugin's session hook, and subagents get it too. Keep it
tool-neutral. Tool-specific rules belong in the tool's own config.

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

# Writing

Before drafting or revising prose, apply the `writing` skill (`/dazi:writing`
in Claude): docs, READMEs, PR descriptions, issue and PR comments, release
notes, emails, commit bodies. It carries the full list of tells.
This covers prose written straight into a reply, not only prose that lands in a
file.

The short version: no bold lead-in bullet lists where a paragraph belongs, no
"not just X, but Y", no em dash doing a comma's job, no closing paragraph that
restates what was already said, and no assistant register ("Certainly!", "I hope
this helps") in anything that isn't chat. Say who does what, or say nothing.

Outward-facing text (PR descriptions, issue and PR comments, release notes) is
read by people who can tell when a maintainer reply was generated. Write it the
way David writes: short, direct, shaped by what it is actually responding to, a
thought that runs on a little, backticked paths, apologizing for a slow reply
when it was slow. Two replies should not share a skeleton. Anything longer than
a one-liner gets shown to David before it posts.
