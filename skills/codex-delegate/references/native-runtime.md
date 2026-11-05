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
  -s danger-full-access \
  -c 'approval_policy="never"' \
  -
```

Runs orchestrated by a Claude session are unsandboxed by default, so Codex can run the full test
suite, local services, Git and package tooling without sandbox failures. The brief states the
boundaries instead: what Codex may write, what it must not touch (other worktrees, devices, shared
services, commits and pushes), and which checks stay with Claude. Use `-s read-only` for a
standalone review that must provably change nothing.

## Git review

Native review accepts custom focus text:

```bash
codex exec review --ephemeral --uncommitted \
  "Adversarially inspect concurrency, cancellation, regressions, and missing tests."
```

Use `--base <branch>` for a branch comparison or `--commit <sha>` for one commit. Run
`codex exec review --help` before relying on less common flags.

## Model and reasoning effort

Set the model and the effort on every run; the defaults don't follow the role. Implementation runs on
the newest Sol model at `xhigh`. A challenge runs on the newest Astra model at `xhigh`, or `max` when
the claim is expensive to get wrong: a load-bearing design, or a correctness or security claim. As of
November 2026 those models are `gpt-6.1-sol` and `gpt-6-astra`. Whatever `~/.codex/config.toml`
sets as the default isn't chosen per role, and an unset effort falls back to the model's catalog
default, which is `low` for `gpt-6.1-sol`.

The catalog changes with each release, so check it rather than trusting the names above. When a
newer Sol or Astra appears, use it:

```bash
codex debug models
```

Effort is model-dependent. Implementing, when the catalog confirms support:

```bash
codex exec --ephemeral \
  -m gpt-6.1-sol \
  -c 'model_reasoning_effort="xhigh"' \
  -s workspace-write \
  - < implement-prompt.md
```

Challenging a design that would be expensive to get wrong:

```bash
codex exec --ephemeral \
  -m gpt-6-astra \
  -c 'model_reasoning_effort="max"' \
  -s read-only \
  "Analyze the proposed design."
```

`max` is the deepest single-agent reasoning. `ultra` adds automatic task delegation and changes the
orchestration semantics, so use it only when that is intended. Do not claim a fixed model ran unless
it was selected and available; `codex exec` prints the resolved model and effort in its header, so
read that back rather than assuming.

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
