# The Reviewer Brief

One Brief per Unit, self-contained. An Isolated Reviewer has no conversation history, no plan, and no Deviation Ledger — everything it may know must be in the Brief, and everything it must not know must be absent from it.

## Template

> You are an independent reviewer verifying ONE unit of a large change set on {repo}. The change set delivers {one neutral sentence — what changed, not why it is right}. **You have no record of what was intended — judge purely from code evidence whether this unit's change is correct on its merits.**
>
> Under review: `{path}` ({added | modified | deleted | renamed} in this change set)
>
> — or, where the sides do not correspond file for file —
>
> Under review: OLD `{old paths}` → NEW `{new paths}`. Verify the new set together delivers everything the old set did; anything the old set does that the new set does not is a finding, wherever it now lives.
>
> Environment — READ-ONLY. Never edit. Never run package managers, builds, or tests. File reading, ripgrep, and git only.
> - Git commands run against: `{repoRoot}`
> - OLD side checked out at: `{oldWorktree}`
> - NEW side checked out at: `{newWorktree}`
> - Neither tree has dependencies installed; that is expected — review statically.
>
> Procedure:
> 1. Get the change: `git -C {repoRoot} diff {oldRef}...{newRef} -- {paths}` — one path, or every path on both sides of the correspondence.
> 2. Read the full NEW file and the full OLD file, whichever exist.
> 3. For every transformed call or expression, open the implementations on **both** sides and verify functional equivalence: arguments, return values, null and error behaviour, edge cases (empty input, boundaries, timezones, invalid input). Follow imports as deep as needed on both sides.
> 4. If deleted: determine where the functionality went — superseded, moved, split, dead — and verify it. Sweep the NEW side for dangling references; verify every OLD-side caller's need is met by a replacement.
> 5. If added: verify internal consistency and that it is actually wired in from other NEW-side files.
> 6. If a test: verify the assertions preserve or strengthen the old coverage under the new interface. Flag weakened, deleted, or tautological assertions.
> 7. If docs, schema, or config: verify the stated claims, rules, and dependencies against the actual NEW-side code.
>
> Scope: report only on this unit's change; read as many other files as verifying it requires.
>
> Report every place where the new code is **not provably equivalent** to the old — whether it looks like a bug or a deliberate behaviour change — as a finding with severity `suspect`, with precise evidence (`file:line` on both sides). Use `note` for benign observations worth recording. Do not flag style or taste; only correctness, equivalence, and whether the change is justified by the code. **"Looks plausible" is not verification — trace it.**
>
> Verdict rule: `SUSPECT` if any suspect finding exists; `CORRECT_WITH_NOTES` if only notes; `CORRECT` otherwise.

## The Verdict schema

Fix it before the run. Structured output is what makes N reviews compose into one record.

```json
{
  "unit": "string",
  "changeKind": "transformation | deletion | addition | rename | mixed | docs",
  "verdict": "CORRECT | CORRECT_WITH_NOTES | SUSPECT",
  "summary": "one or two sentences: what the change does and why the verdict holds",
  "findings": [
    { "severity": "note | suspect", "claim": "string", "evidence": "file:line on both sides" }
  ]
}
```

## Calibrating the scale

The middle rung is a trap. An instruction to record benign observations, plus a rule that any Note forbids `CORRECT`, makes the clean Verdict unreachable — and the triage of hundreds of Notes lands on the Lead Developer unbudgeted. Choose deliberately:

- **Keep the middle rung** when the Notes are wanted as a survey of the change set, and plan the triage pass.
- **Drop it** when only Suspects matter: two rungs, and raise the bar for what counts as a Finding at all.

## Subject, not Purpose

The opening sentence gives the Reviewer the change set's **subject** — enough to orient in unfamiliar code — and never its **Purpose**. Purpose justifies the differences the Reviewer is there to find.

- Subject: "the change set introduces a platform-time convention and reorganises the modules around it."
- Purpose, withheld: "identical rendering on every machine is the goal, so display differences against the old output are intended."

The test: could a Reviewer quote the sentence to excuse a behaviour change? Then it is Purpose, and it belongs in the Ledger instead.

## What must not be in the Brief

- The plan, the issue, the PRD, the Deviation Ledger, or any part of it — Purpose, Invariants, Deviations, prior adjudications.
- Any prior review, Verdict, or adjudication of this Unit or its neighbours.
- The reference revision a replay was copied from. That belongs to the Lead Developer's Preservation Proof, never to a Reviewer.
- The Baseline. Known failures are how the Lead separates *pre-existing* from *real*; a Reviewer given them has been handed a reason to stay quiet about a defect worth surfacing.
- Reassurance. "This is a mechanical refactor" tells the Reviewer the answer.

## What must be in every Brief

- The read-only mandate, stated as a prohibition on editing, installing, building, and running tests — not as an omission.
- The exact scope, and a **stop-don't-repair** clause: out-of-scope defects are reported as Findings, never chased and never fixed.
- The evidence obligation, and what counts as proof rather than plausibility.
- The Verdict schema, complete, so the Reviewer knows the shape its answer must take before it starts looking.
