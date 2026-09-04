---
name: codex-delegate
description: Use when authoring ultracode workflows or when the user explicitly asks Claude to delegate implementation, review, analysis, or verification to Codex or GPT.
---

# Delegating work to native Codex

Use the native `codex` executable as an alternate model for a coherent work item. Cross-model
diversity is most useful when one model produces work and the other challenges it.

## Routing

User instructions determine the role and model when explicit. Otherwise, choose one default per
work item:

1. **Codex implements** a self-contained, decision-complete change; Claude independently reviews
   and verifies it.
2. **Codex challenges** a plan, claim, diff, or implementation produced by Claude.
3. **All Claude** for trivial work, context tightly coupled to the current Claude session, or when
   native Codex is unavailable.

Prefer Codex on only one side because that preserves an independent cross-model check. This is a
default, not an override of user intent. If the user explicitly requests Codex for both
implementation and review, comply and state that the second pass is self-review rather than an
independent cross-model review.

## Use native Codex directly

Outside a dynamic workflow, call the `codex` executable directly. Inside a dynamic workflow, use
the native `codex-runner` custom agent as a thin transport adapter. Its name alone is not proof;
require its preflight evidence, process exit code, and native Codex output.

Preflight the native runtime itself:

```bash
command -v codex >/dev/null && codex login status
```

If an explicit Codex request cannot run, report the failure instead of silently substituting
Claude. For automatic routing, falling back to all-Claude is acceptable when disclosed.

## Quick reference

| Work | Native entry point |
|---|---|
| Implement or analyze | `codex exec --ephemeral` |
| Review Git changes | `codex exec review --ephemeral` |
| Standalone adversarial check | `codex exec --ephemeral -s read-only` |
| Dynamic workflow stage | `agent(..., { agentType: 'codex-runner' })` |
| Machine-readable verdict | add `--output-schema <file> -o <file>` |
| Inspect current model capabilities | `codex debug models` |

Read [references/native-runtime.md](references/native-runtime.md) for command templates, effort
selection, permissions, ephemeral semantics, and structured output. Read
[references/workflow-integration.md](references/workflow-integration.md) when wiring Codex into a
Claude workflow.

## Verification contract

- Pass prompts through stdin or a prompt file; never interpolate untrusted prompt text into a shell
  command.
- Set sandbox and approval behavior explicitly for unattended runs.
- Do not put max-effort Codex behind a finite Bash timeout. In dynamic workflows, the
  `codex-runner` launches it with persistent Monitor and waits for process exit.
- Treat tests, compilers, type checkers, and linters as the primary verification. LLM review is
  complementary, not authoritative.
- Inventory Codex's actual output and working-tree changes before reporting what happened.
- Surface model disagreements rather than averaging them. Prefer a deterministic check; otherwise
  escalate after roughly three implement/review rounds.
- The calling Claude workflow owns background execution, concurrency, isolation, rollback, and
  recovery policy. This skill intentionally does not prescribe them.

## Common mistakes

- Treating `--ephemeral` as a privacy guarantee. It only promises that local session rollout files
  are not persisted.
- Assuming effort names are universal. They are model-dependent; inspect the current model catalog.
- Leaving effort unset and assuming a deep run. The current default model reasons at `low` unless
  told otherwise, so an adversarial pass without an explicit effort is shallow and looks fine.
- Prompting for ad-hoc JSON and scraping stdout when native `--output-schema` and
  `--output-last-message` can provide a stable contract.
- Claiming a fixed model ran without passing `--model` and verifying that it is available.
- Launching or polling a max-effort run with Bash's finite timeout instead of persistent Monitor.
- Silently answering as Claude after an explicit request to use Codex.

For load-bearing design work (new subsystems, architecture changes, anything where a
wrong design is expensive), follow the position-paper / adversarial-review / settled-
disagreements procedure in [references/design-loop.md](references/design-loop.md).

The rationale for the cross-model default is summarized in
[references/evidence.md](references/evidence.md).
