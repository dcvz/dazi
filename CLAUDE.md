# Workflows

David, writing directly. This section is my standing opt-in for Claude Code's Workflow
tool: I am asking for multi-agent orchestration on the work described here, once, for
every session. Don't stop to ask again, and don't rule a task out because I didn't say
the magic word that day.

Use it when the work genuinely splits into independent pieces that each need real
reading, like auditing a subsystem, reviewing a large diff dimension by dimension,
sweeping a repo for a pattern, or porting a call site at a time. A decision I'll have to
live with across the codebase is worth several competing answers before I see one. So is
a claim that's expensive to be wrong about, which should face a pass built to break it
rather than one built to confirm it. Reading that exceeds a single context is the obvious
case.

Stay solo when the work is local or when each step depends on the last. Stay solo when
you can already see the answer, and leave ordinary conversation as conversation. One
agent that finishes beats ten that repeat each other.

A handful of agents is the default size. Ultracode is still my word for the big ones. Say
what you're launching and how wide as it starts. When the work-list isn't known yet, scout
inline first and fan out over what you find, and load the `workflow-authoring` skill
before writing the script.

# Working with Codex / GPT-5

When authoring any workflow, or when the user asks to use Codex/GPT, invoke the
`codex-delegate` skill and follow it.

In dynamic workflows, route a Codex work item through `agentType: 'codex-runner'`. Give that agent
an exact native Codex command whose non-trivial prompt is already stored in a file; do not set the
workflow agent's `model` option. Outside workflows, invoke the native `codex` CLI directly.

Core policy: use the native `codex` CLI directly. Per work item, default to Codex as either the
implementer or an adversarial challenger, with Claude on the opposite side, or keep the work all
Claude when cross-model delegation adds no clear value. Explicit user instructions take priority;
if they put Codex on both sides, comply and disclose that the second pass is not an independent
cross-model check.
