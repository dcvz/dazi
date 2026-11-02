---
name: engineering
description: Keep a change to the least code that does the task, and review a diff for over-abstraction before it is committed. Use before committing code an agent wrote, when reviewing or cleaning up a diff, and when a change has grown larger than the task needs.
---

# Engineering

Write the least code that does the task, in the way the code around it is already written.

## Scope

The caller's files or diff. Otherwise the current diff against the base branch (default
`main`), including the working tree.

If you wrote the change yourself, spawn a subagent (in Claude Code, `Explore` or
`general-purpose`) to find what to cut. Authors defend their own lines; a fresh reader does
not. Pass it the scope and this file.

## The over-abstraction test

Read every changed line against the code around it. The change passes when a maintainer of
this repo couldn't tell which lines an agent wrote. It fails on any layer, option or comment
the neighbouring code wouldn't have.

## Cut

- Wrappers that only forward a call, and options or flags every caller sets the same way.
  Inline them.
- Defence against cases that can't happen: checks on values the types already rule out,
  fallbacks for input no caller sends, a `try`/`catch` that only rethrows or logs.
- Casts to `any`, or the language's equivalent, that only get past the type checker.
- Deep nesting that early returns would flatten.
- Logic that duplicates something the codebase already has. Call the existing code.
- Tests that only assert themselves, such as a mock checked against its own return value.
- Edits the task didn't need: reformatting, renames, reordering.
- Docs the change made stale. Fix them or delete them.
- Comments. Run the `prune-comments` skill over them.

## Keep

A layer stays when it is a deliberate seam with a named reason: a network or process
boundary, a seam the tests actually use, a public API. Otherwise add the layer when its
second user arrives.

Before cutting, search for the callers, config and tests that use it, and keep what they
need.

## Large changes

Have one or more subagents list cuts, then have another try to refute each one against the
code. Apply only the cuts that survive, as written, then run the tests. Fixes the feature
doesn't depend on go in their own commit or PR, ahead of it.

## Report

What was cut, by kind, with a count. What was kept and its reason, one line each. Anything
left for a person to decide.
