# Cross-model design loop

The procedure for load-bearing design and implementation work with Codex as the
counterpart. Use max reasoning effort for the adversarial stages.

1. Position paper first. Write your own design as a falsifiable position: ranked goals,
   the mechanism, rejected alternatives with reasons, and numbered open questions you
   genuinely cannot settle. Ground it in a fact inventory (file:line) — dispatch a
   read-only Codex inventory pass first when the subsystem is large.
2. Adversarial review at max, read-only. Instruct: "answer the open questions with
   positions, not options", "attack the design against every consumer", "state explicit
   disagreements separately from refinements", "end with the converged design in one
   page + ordered work items". Point it at the real code and any captured evidence
   (logs, harness artifacts) — its best findings come from citing your own artifacts.
3. Settle every disagreement explicitly, in writing, before implementation. Accept or
   override each with a reason; never average positions. Record the settled list in the
   implementation spec ("the disagreement list is settled in the review's favor" or
   itemized overrides).
4. Verify review claims in code before acting on them. Plausible-sounding findings can
   be wrong or already-fixed; each claimed defect gets a file:line check or a quick
   instrumented run before it drives work.
5. Implement via Codex with the converged design as normative authority, failing-first
   tests where the design names guarantees. The implementer never runs builds/tests —
   the caller verifies everything independently (suites + harness), reviews the full
   diff, and repairs small issues directly rather than round-tripping.
6. The acceptance harness closes the loop: red before, green after, with the review's
   verification table translated into exact assertions. A review that produced no new
   failing check probably wasn't adversarial enough.

Escalation: after ~3 implement/review rounds without convergence, stop and surface the
disagreement rather than iterating further.
