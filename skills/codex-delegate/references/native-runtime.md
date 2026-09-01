# Native Codex runtime

Use the installed `codex` executable. Its `--help` and model catalog are the current local source of
truth.

## Preflight

```bash
command -v codex >/dev/null || { echo "codex is not installed" >&2; exit 1; }
codex login status
codex --version
```

For an explicit Codex request, stop and report a failed preflight. For automatic routing, disclose
the failure before falling back to Claude.

## Implementation or analysis

Pass the prompt on stdin. An explicit approval policy prevents a headless run from waiting for a
decision:

```bash
printf '%s' "$SPEC" | codex exec --ephemeral \
  -s workspace-write \
  -c 'approval_policy="never"' \
  -
```

Use `-s read-only` for analysis. Grant only the permissions the task needs. A command that needs
more permission will fail under `approval_policy="never"`; report that failure rather than silently
broadening access.

## Git review

Native review accepts custom focus text:

```bash
codex exec review --ephemeral --uncommitted \
  "Adversarially inspect concurrency, cancellation, regressions, and missing tests."
```

Use `--base <branch>` for a branch comparison or `--commit <sha>` for one commit. Run
`codex exec review --help` before relying on less common flags.

## Model and reasoning effort

Leave model and effort unset unless the user or workflow needs a specific choice. Inspect current
capabilities rather than copying a stale list:

```bash
codex debug models
```

Effort is model-dependent. For example, when the catalog confirms support:

```bash
codex exec --ephemeral \
  -m gpt-5.6-sol \
  -c 'model_reasoning_effort="max"' \
  -s read-only \
  "Analyze the proposed design."
```

`max` means the deepest single-agent reasoning. `ultra` may enable automatic delegation and changes
the orchestration semantics; use it only when that is intended. Do not claim a fixed model ran
unless it was selected and available.

## Structured output

Use a JSON Schema for the final response and write the final message separately from progress:

```bash
codex exec --ephemeral \
  -s read-only \
  --output-schema "$SCHEMA" \
  -o "$RESULT" \
  - < "$PROMPT_FILE"
```

Check the process exit code before parsing `$RESULT`. A nonzero exit is an execution failure, not a
schema mismatch to paper over.

`--json` changes stdout into a JSONL event stream. It is useful for progress and diagnostics and can
be paired with `-o`; it is not a replacement for `--output-schema` when downstream steps require a
stable final object.

## Ephemeral semantics

`--ephemeral` prevents persistence of local session rollout files. It does not promise that no
provider-side operational or account records exist. Describe only the documented local guarantee.
