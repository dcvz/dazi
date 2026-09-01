# Working with Codex / GPT-5

When authoring ultracode workflows, or when the user asks to use Codex/GPT, invoke the
`codex-delegate` skill and follow it.

In dynamic workflows, route a Codex work item through `agentType: 'codex-runner'`. Give that agent
an exact native Codex command whose non-trivial prompt is already stored in a file; do not set the
workflow agent's `model` option. Outside workflows, invoke the native `codex` CLI directly.

Core policy: use the native `codex` CLI directly. Per work item, default to Codex as either the
implementer or an adversarial challenger, with Claude on the opposite side, or keep the work all
Claude when cross-model delegation adds no clear value. Explicit user instructions take priority;
if they put Codex on both sides, comply and disclose that the second pass is not an independent
cross-model check.
