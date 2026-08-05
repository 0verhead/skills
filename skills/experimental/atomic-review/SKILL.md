---
name: atomic-review
description: Roles and rules for verifying that a large change set is correct — Isolated Reviewers who never see the intent, and a Lead Developer who charts that intent from artifacts and adjudicates every Finding against it. Review only; it produces adjudicated Findings, never fixes. Use when a refactor, replay, migration, or large PR must be verified change by change, when Findings must be adjudicated before anyone acts on them, or when another skill needs the isolation-and-adjudication vocabulary.
---

# Atomic Review

Verify a large change set by **separating evidence from judgment**: many Isolated Reviewers establish what the code does, one accountable Lead Developer rules on what it means. The Reviewers are denied the record of what was intended — that is the design, not an oversight: the record is the one thing that must never be used to confirm the change happened. Use this language and these rules wherever a change set is too large to hold in one head and too consequential to accept on assertion.

**Review only.** Nothing here implements, commits, or repairs — the adjudicated list goes to a fix loop.

## Roles

**Lead Developer** — the single role accountable for the correctness of the whole change set. Charts the intent, adjudicates every Finding, re-derives every Gate, answers for the outcome. May delegate labour; may never delegate judgment. Sole holder of the Deviation Ledger. _Avoid_: orchestrator, coordinator — they name the mechanics and drop the accountability.

**Isolated Reviewer** — one agent, one Unit, no knowledge of intent. Sees the two sides and the history behind them; not the plan, the Ledger, or any prior review. Produces Findings, never fixes and never taste. Its isolation is the source of its evidential value: it cannot be talked into a change it was never told to expect. _Avoid_: reviewer unqualified — the isolation is the method, and the bare word spends it.

**Principal** — the party whose sanction is required before anything is acted on. Sets scope, rules wherever correctness trades against product effect, and decides which Findings become work.

## Glossary

**Unit** — the smallest part of a change set one Isolated Reviewer verifies end to end. Where the sides correspond file for file it is one changed file; where they do not, it is one Correspondence. _Avoid_: task, chunk — both invite a decomposition of your own, which cuts across the evidence.

**Correspondence** — what on the New side stands in for a given part of the Old side. A refactor within one tree supplies it in the diff; a port across languages, frameworks, or repositories must have it built, and building it is where dropped behaviour surfaces. An Old-side element with no Correspondence is a Finding, not a gap in the map.

**Old side** and **New side** — the two pinned, read-only source trees a Unit is verified between; two revisions of one repository, or two repositories entirely. They must be walkable, not a diff: a diff shows what moved, never what the code now means. _Avoid_: base, head, before, after — diff words name the artifact, not the two states being compared through it.

**Brief** — the self-contained instruction an Isolated Reviewer receives: the scope, the read-only mandate, the evidence obligation, and the Verdict schema. _Avoid_: prompt — a prompt is phrasing, a Brief is a contract.

**Finding** — a claim that the New side is not provably equivalent to the Old side, carrying citations on both. _Avoid_: issue — a Finding may become one, but not before adjudication.

**Suspect** — the severity that decides a Verdict: the Reviewer could not establish equivalence and will not vouch for the difference. _Avoid_: bug, defect, blocker — an Isolated Reviewer has no standing to call it any of those.

**Note** — the severity that decides no Verdict. Notes are a survey of the change set, and someone must be budgeted to read them.

**Verdict** — what an Isolated Reviewer returns for its Unit, in a schema fixed before the run. The schema is what makes N reviews compose into one record instead of accumulating as N essays.

**Artifact** — a source that cannot drift: a commit, a diff at a pinned revision, a PR body, issue text, CI output. _Avoid_: source of truth — overloaded, and it claims more than an Artifact carries.

**Pointer** — a recollection naming which Artifact to open: a memory file, a handoff, a prior summary. _Avoid_: evidence, context — the entire point of the term is that it is neither.

**Deviation Ledger** — the Lead Developer's cited chart of the intent, in five parts (see *Charting the intent*). Derived from Artifacts, never standing in for them. Marks a deviation *intended*; never *settled*.

**Preservation Proof** — a byte-level comparison against a pinned reference establishing that a flagged Unit is unchanged from already-reviewed content, or differs from it exactly by a named deviation.

**Gate** — a mechanical check that answers with an exit code rather than an opinion. In review it is an instrument of refutation: a Finding that predicts a failure the Gate does not produce is dead.

**Disposition** — what adjudication returns for each Finding: *intended*, *pre-existing*, *refuted*, or *real*. Only *real* leaves the review.

## Charting the intent

The Lead charts the intent **before any Verdict arrives**. A chart drawn afterwards is not a record; it is a rationalisation, and it will ratify whatever the Reviewers happened to find.

The Ledger has five parts, and they do not point the same way. Purpose and Deviations can show a difference was sought; Baseline can show a failure predates the change; **Invariants do the reverse — a Finding that breaks one is convicted, not excused**; and prior adjudications inform without ever concluding. The Ledger is what you rule *against*, never what rules *for* you.

- **Purpose** — what the change set is *for*. It names where the change is *supposed* to differ, and so it overrides the equivalence default: "the same rendering on every machine is the goal, not equivalence to the old output" disposes of a whole class of Findings that are otherwise indistinguishable from defects. Without it, a difference that is correct but serves no purpose reads as intended rather than as scope creep.
- **Invariants** — what must be left identically alone, recorded as a *keeps* clause on the entry they bound — *keeps the customer scoping*, *keeps the published config version*, *keeps main's structure* — not as an abstract list. Then state the one global invariant explicitly, because it is the one everybody assumes and nobody writes: usually *this preserves what the system does today*.
- **Deviations** — one entry per place the New side deliberately does not preserve the Old, each citing the Artifact that authorises it.
- **Prior adjudications** — rulings from an earlier review of this content. They may inform an adjudication; they may never conclude one.
- **Baseline** — the Gate results and the **known failures** at the Old side. Without it, *pre-existing* is indistinguishable from *real*, and two of the four Dispositions collapse into one.

**The Ledger is a closed world: anything it does not cover is a live Finding.** No unsourced section, no to-verify bucket — a bucket is where Findings go to be parked. An uncited difference is adjudicated on merits like any other. That is also what routes an undecided question to the Principal, instead of letting the Lead invent an intent for it.

Chart it in this order:

1. **Frame.** Resolve both refs, confirm the change set is non-empty, capture the Baseline. A bad ref fails here, not inside N Reviewers.
2. **Search the Artifacts in order** — issue references in the commit messages; the PR body; the change set's own commit messages, where deviations are usually named individually; a linked spec; the reference revision, if this is a replay. Cite what you use. Where two Artifacts disagree, the later supersedes — and the disagreement is itself a Finding, because someone's understanding moved and it may never have reached the code.
3. **Read the standing record.** The glossary and the decision records covering the touched area bound what counts as a deviation at all: conforming to a recorded decision is not one.
4. **Ask the Principal only what the Artifacts leave open**, and **write each answer back into an Artifact** before it enters the Ledger. An answer that lives only in the Ledger has smuggled a Pointer in as evidence.

A change set with no recoverable intent is still reviewable. The Ledger holds only its Baseline, every difference adjudicates on merits, and the report says so.

## The asymmetry

```
     the Deviation Ledger · the plan · any prior review
                      │
                      ▼
          ┌───────────────────────┐
          │    Lead Developer     │   rules on meaning
          └───────────┬───────────┘
                      │   Findings, evidence, Verdicts
      ┌───────────┬───┴───────┬───────────┐
      │           │           │           │
   Reviewer    Reviewer    Reviewer    Reviewer   establish fact
      │           │           │           │
          Old side and New side, nothing else
```

Briefs flow down, evidence flows up, intent never reaches the bottom row.

## Rules

- **Isolate the Reviewer from the intent.** Whoever knows what a change was supposed to do cannot independently confirm that it does it. The Ledger stays with the Lead for the same reason: a Reviewer shown which deviations were sanctioned stops finding them.
- **Chart before you run.** The Ledger is built against the change set, not against the Findings.
- **Enumerate from the Old side.** A change set's footprint proves what was touched, never what was left behind: behaviour dropped rather than moved appears in no diff, so a review that derives its Units from the diff alone can return clean on a port that lost a feature.
- **Intended is not settled.** Provenance answers whether a difference was deliberate, never whether it was right. A prior adjudication is a record, not an oracle — and the Lead, not the Reviewer, is the one exposed to this error. A Ledger that lets prior rulings *override new Findings of the same shape* has automated the mistake.
- **A match is not a disposal.** Confirm the Finding *is* the entry rather than merely resembling it, and rule on the merits it raises.
- **Artifacts are evidence; Pointers are addresses.** What a Pointer claims and no Artifact supports does not enter the Ledger.
- **Narrow the reporting scope, not the reading scope.** Report on one Unit; read as far as the trace requires.
- **Every scale needs a reachable top.** If the clean Verdict cannot be earned, the scale is decoration and its triage relocates silently to the Lead.
- **State the evidence obligation.** "Looks plausible" is not verification. Demand the trace and citations on both sides, or receive confident prose.
- **False positives are the fee, not the failure.** A Finding costs one adjudication; a miss costs the change set. A Reviewer that independently rediscovers why a change was made has produced better evidence than the record.
- **Escalate on evidence.** Run narrow until real Verdicts prove the Brief, then widen. The first results validate the instrument.
- **Refute where you can.** A Gate, a CI artifact, or a pinned diff that contradicts a Finding settles it faster and harder than argument.
- **Fan-out buys traces; judgment does not fan out.** Budget the serial layer — the work concentrates there.
- **Reserve the Principal's decisions.** Scope, sanction, and correctness-against-product calls stay with the Principal even when the evidence looks decisive. Say what the evidence shows, then stop.
- **The review must outlive the run.** A real Finding that exists only in a transcript did not happen. It lands as an issue with a failure scenario and a verify recipe, pinned to a revision, or it is lost.

## Run shape

1. **Frame and chart** — see *Charting the intent*. Nothing runs until the Ledger exists.
2. **Enumerate.** Establish the Correspondence across both sides and derive the Units from it. Anything on the Old side without one is already a Finding. Show the skip list to the Principal for veto before anything runs.
3. **Brief.** One Brief per Unit — see [REVIEWER-BRIEF.md](REVIEWER-BRIEF.md).
4. **Run narrow, then escalate.**
5. **Adjudicate.** Rule on every Suspect against the Ledger, establishing the fact yourself — a Preservation Proof where a reference revision exists, a Gate where the claim predicts a failure, a read of both sides otherwise. Triage every Note. Assign each a Disposition and record what disposed of it.
6. **Dispose.** Present the *real* Findings to the Principal, separated into in-place fixes and follow-ups. Everything sanctioned goes to a fix loop; everything else is filed. The review ends here.

## Executing the run

One question belongs to the Principal, asked before anything launches: **which reviewer — model and effort as a single choice.** *Opus 5 at medium.* *GPT-5.6-sol at xhigh.* The two settings trade against each other rather than compose, so they are answered together: a stronger model held at low effort and a lesser one driven hard are different points on the same curve, and what the curve buys is how far a Reviewer follows an import chain before it settles for plausible.

```
reviewer := ASK the Principal        # model and effort as one answer,
                                     # e.g. "Opus 5 · medium" or "GPT-5.6-sol · xhigh"

IF a Workflow tool EXISTS:
    script one agent() per Unit — the Brief, the forced Verdict schema, model, effort
    run a narrow first batch        -> read those Verdicts before widening
    resume the same run, widened    -> completed Units replay from cache
ELSE:
    one peer-CLI invocation or one subagent per Unit, same Brief and same schema
    write each Verdict to its own file as it lands

adjudicate from the collected Verdicts, never from the transcript
```

Naming the Workflow tool here is what authorises its use for the Reviewers; nothing else in this skill spawns anything. Either way the Verdicts land outside the Lead's context — N review reports read into the conversation is the failure the schema exists to prevent.

## Rejected framings

- **Redundancy as isolation.** N Reviewers on one Unit with the same knowledge is a poll; one Reviewer per Unit denied the record is a control. Voting cures variance, isolation cures bias.
- **The informed reviewer as the efficient one.** Briefing Reviewers on intent removes the Findings, and with them the only property that made the Verdict worth having.
- **The Reviewer proposing the remedy.** Evidence and repair in one role produce Findings shaped to their fix.
- **The Lead as router.** A Lead that forwards Findings has kept the title and given away the role.
- **Faithfulness as correctness.** That a change reproduces what an earlier reviewed change did is provenance. The earlier change can be wrong, and a replay carries its defects forward intact.
- **Fleet size as rigor.** Agent count measures coverage. Rigor is measured at the adjudication.
