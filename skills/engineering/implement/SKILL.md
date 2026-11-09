---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Call the Skill tool with "tdd" where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, call the Skill tool with "code-review" to review the work. Changing the code changes the model, so read each finding as a symptom: group them by cause and seam, within and across rounds. A finding holds when fixing it improves the code. Fix the cause in the model, not the lines, even if it means starting that part over. Take the shape from the model and the decisions already made (spec, ADRs, earlier rounds), never from the fixes so far: would you build it this way today? Fix the findings that hold, then review the whole change again until a round has none that hold.

Commit your work to the current branch.
