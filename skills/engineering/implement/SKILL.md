---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Call the Skill tool with "tdd" where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, call the Skill tool with "code-review" to review the work. Findings or fixes that keep landing in one area are a shearing pattern: find the conceptual contour that explains it. If you wouldn't build that part in this shape from scratch, fix the model, not the lines: make the change easy, then make the easy change. Check each fix against decisions already made (spec, ADRs, earlier rounds), since reviewers see only the diff. Fix the findings that hold, then review the whole change again until a round has none that hold.

Commit your work to the current branch.
