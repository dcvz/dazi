# Claude workflow integration

## Native runner

Route Codex stages through the user-level `codex-runner` custom agent:

```js
const result = await agent(
  `Run this exact native Codex command and relay its result:\n\n` +
  `codex exec --ephemeral -C ${repo} -s workspace-write ` +
  `-c 'approval_policy="never"' ` +
  `-c 'model_reasoning_effort="max"' ` +
  `--output-schema ${schemaFile} -o ${resultFile} - < ${promptFile}`,
  {
    label: 'codex:implement',
    phase: 'Implement',
    agentType: 'codex-runner',
    schema: RUN_RESULT,
  },
)
```

Do not set a workflow `model` option on this `agent()` call; allow the `codex-runner` definition to
select its cheap Claude transport model. The actual coding model and effort belong in the native
Codex command.

The workflow must prepare the prompt and schema files before invoking the runner. The runner has
only transport access and intentionally cannot write the prompt itself. It uses Bash for preflight
and persistent Monitor for the native Codex process. Put non-trivial or untrusted prompt text in
the prompt file rather than interpolating it into the shell command.

Do not add `run_in_background`, a finite timeout such as `600000`, or a Bash polling loop to the
runner prompt. Max-effort Codex runs can legitimately exceed Claude Code's Bash ceiling; the
runner's persistent Monitor watch ends only when the native process exits.

Keep the native prefix exactly `codex exec --ephemeral` or `codex exec review --ephemeral`. The
user-level permission rules allow those prefixes and deny Codex's dangerous bypass flags and
`danger-full-access`; other command shapes may pause for approval.

## Role contract

For automatic routing, assign Codex one role per coherent work item:

- Codex implementation -> Claude review and deterministic verification.
- Claude implementation -> Codex adversarial review and deterministic verification.
- No clear cross-model benefit -> all Claude.

Explicit user instructions take priority. If the user asks Codex to implement and review the same
work, run both and label the latter as self-review rather than an independent cross-model check.

## Prompt handoff

Give Codex the artifact, decision-complete task specification, constraints, and success criteria.
Do not include Claude's hidden reasoning or rationalizations; that anchors the challenger.

Pass prompts through stdin or a file:

```bash
codex exec --ephemeral -s read-only - < "$PROMPT_FILE"
```

Never splice arbitrary prompt text into a shell command. Shell substitutions, backticks, and dollar
expansion can execute or corrupt it.

## Result handling

The wrapper or calling workflow must capture:

- Exit code
- Final message, preferably through `-o`
- Stderr or JSONL diagnostics when relevant
- Touched files and Git state for write-capable runs
- Deterministic verification results

If Codex asks a blocking question, surface it to the user. Continue only after receiving the answer;
do not guess. A new ephemeral pass may read the current artifact state and the answer.

Use `--output-schema` when another workflow stage consumes the result. Treat nonzero process exit as
failure before parsing the output object.

The runner's successful return proves only that the native process completed. A later workflow
stage must still inventory the working tree and run deterministic verification.

## Verification and disagreement

Deterministic checks are primary. Cross-model review catches different failure modes but is not a
sound oracle. Present disagreements explicitly and resolve them with a test or user decision. Avoid
open-ended critique loops; escalate after roughly three rounds without convergence.

## Ownership boundary

The calling Claude workflow owns scheduling, concurrency, working-tree isolation, rollback,
cleanup, and recovery. The runner owns only the lifetime of its native Codex child through
persistent Monitor. This skill supplies Codex routing and invocation contracts only.
