---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Call the Skill tool with "tdd" where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, call the Skill tool with "code-review" to review the work.

Read findings as symptoms: group them by cause and seam, within and across rounds. A finding holds when fixing it improves the code. Fix the findings that hold at their cause, not their lines, then review the whole change again until a round has none that hold. A finding that recurs across changes is a missing standard: propose it for the standards the reviewers read.

Stop and put the approach to the user when a round flags an area an earlier round flagged, and in any case by the third round. Changing the code changes the model, so take the shape from the model and the decisions already made (spec, ADRs, earlier rounds), never from the fixes so far. Would you build it this way today? One concept, one home; one rule, one place to change. If not, reshape it, even if it means starting over.

Commit your work to the current branch.
