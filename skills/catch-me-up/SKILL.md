---
name: catch-me-up
description: Bring the user back up to speed after time away with what happened, what's waiting on them and where things stand, checked against the live state rather than recalled. Use when they ask to be caught up or for a summary or status of the work, say they've been away or out of the loop, or come back to a long-running session.
---

# Catch me up

The user is back and wants to know where things are without rereading the conversation.
Everything earlier in the conversation, in a summary of it, or in memory notes is a claim
about the past. Check it before you repeat it.

## Scope

Start from the conversation and the repo it works in. If the conversation was compacted,
its summary is where the history starts. When the user names something broader, such as a
project or a set of PRs, cover that instead. Ask only when you can't tell what they've been
out of.

## Gather

Look at whatever the work touched:

- the conversation, and any notes or memory your setup keeps for the project
- git: branches and worktrees, uncommitted or unpushed changes, stashes
- PRs: opened, merged or closed since, review comments, CI
- issues, deploys or releases the work was aimed at
- anything still running: background jobs, subagents, scheduled tasks

Then check each thing you're about to say against the live state. A PR that was open may
have merged, a red build may have gone green, someone else may have pushed. Report what is
true now, and say when you couldn't check something.

## What to say

Open with where things stand, in a sentence or two.

Next, what's waiting on the user: decisions, approvals, reviews, and steps only they can
take, like one on their own machine or in an account only they hold. Give each, in a
sentence or two, enough to decide without scrolling back: the options and the one you'd
pick. They can't get this part anywhere else, so it comes before the history.

Then what happened, as outcomes: merged, shipped, broke, decided, dropped. Leave out the
commands, the dead ends unless one changed the plan, and the routine: automated commits,
green runs nobody was waiting on. Say what failed or was skipped as plainly as what worked.

End with what's still running and when it should finish. If the next step is already
approved, start it and say so rather than describing it as next.

## Shape

Follow the `writing` skill. Catch-ups are where bold lead-in status lists creep in most.
Don't label the parts: no bold "Waiting on you:" or "What happened:", since the order
already says which part the reader is in. Write paragraphs, and keep lists for real sets,
such as several decisions or a history that spans weeks, at a line per item.
Size it to what changed: after an hour and one merged PR, two sentences will do. Use
absolute dates, and give links, paths and IDs they can open. Stop after the last item.
