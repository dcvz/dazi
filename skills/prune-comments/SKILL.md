---
name: prune-comments
description: Delete AI comment slop from a diff or set of files. Use when the user says comments are too verbose, asks to clean/prune/tidy/audit comments, or before committing code an agent wrote.
---

# Prune comments

Comments written by an agent are guilty until proven innocent. Delete first; keep only
what survives the test below.

## Scope

The caller's files or diff. Otherwise the current diff against the base branch
(default `main`), including the working tree. Only touch comments inside that scope.

If you wrote the code under review yourself, spawn a subagent (`Explore` or
`general-purpose`) to produce the kill list before editing. Authors defend their own
comments; a fresh reader does not. Pass it the scope and this file. Do not restate
the rules to it.

## The test

For each comment, in order. First match wins.

1. **Narration** — restates the code, the signature, or the step (`// loop over users`,
   `// returns the id`, banners, `// end of class`). Delete.
2. **Change note** — addressed to a reviewer or written against the diff
   (`// now uses cache`, `// fixed race`, `// keeping old behavior for now`,
   commented-out code). Delete.
3. **Moving target** — points at an ephemeral doc, plan file, section number, or
   "see the spec". Delete, or rewrite to carry the substance inline. Ticket IDs and
   stable maintained docs may stay.
4. **Bloated why** — real reason buried in throat-clearing. Cut to the one clause that
   stops someone breaking it. Occam's razor applies to survivors, not just to deletions.
5. **Stale** — describes code that no longer exists. Delete.
6. **Real why** — a non-obvious constraint imposed by something outside this codebase
   (platform, vendor, protocol, browser bug, wire format). Keep.

When unsure whether a keep clause applies, delete.

## Always keep

- Legal and license headers.
- Doc comments that define a public API contract.
- Issue or RFC links explaining a constraint the code cannot express.
- `// prettier-ignore` and formatting pragmas.
- `TODO`s. They mark work; they are not a substitute for it.

## Looks deletable, is not

- Cross-file pointers ("callers in X assume this stays sorted").
- Semantics of a data literal that nothing else records (units, encoding, ordering).
- Format or wire contracts ("bytes 4-8 are big-endian").

## Suppressions

`@ts-ignore`, `@ts-expect-error`, `eslint-disable`, `#[allow(...)]` and friends are not
comments to prune, they are debt to report. Look up the rule. If it guards correctness
or safety, flag the exact symbol as needing a real fix rather than silently deleting or
keeping the suppression.

## Constraint comments

A comment that says "do not remove" or "do not change wording" is asserting a rule the
code does not enforce. Offer the cheapest enforcement in scope: a type, an assertion, a
test, or a lint rule. If the user approves, encode it and delete the comment. If not,
delete the comment and report the constraint as unenforced.

## Rules

- Never write application code here beyond deleting comments. If a comment exists only
  because the code is unclear, flag the symbol for a rename or extract; do not do it in
  this pass.
- Behavior must not change. `git diff` should show comment lines only.

## Report

Deletion count, files touched, comments kept and why (one line each), symbols flagged
for reshaping, suppressions found, unenforced constraints. Do not list every deletion.
