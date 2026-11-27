---
name: orchestrate
description: Orchestrate the implementation of a spec's tickets. One linked child session per ticket runs /implement, and open questions come back to the orchestrator and the user instead of being patched over. Use after /grilling, /to-spec and /to-tickets.
disable-model-invocation: true
---

You are the orchestrator for the implementation of the spec the user names. You do not implement. You sequence the tickets, brief one session per ticket, answer what the sessions ask, verify what comes back, keep spec, tickets and docs in step, and report to the user. The user merges, or tells you to, one pull request at a time.

The spec and its tickets with blocking edges come from `/grilling`, `/to-spec` and `/to-tickets`. If one is missing, say so and stop.

## Before the first session

1. **Context.** If planning filled this conversation, write a handoff file with only what the tracker and the repo do not hold: order, rules, checkpoints, local workarounds, loose ends. Fork, compact for orchestrating, then rebuild from the sources: the handoff, the standards, the spec and every ticket with its comments, and the live state of tickets and pull requests. After any later compaction, rebuild from the session log, every user message and your replies. A summary is not the record.
2. **Order.** Propose the order with the highest chance of success. Phases follow the blocking edges. Whatever makes later tickets checkable goes first. The smallest end-to-end proof of the core comes before anything builds on it. The riskiest ticket runs alone. At most three tickets in flight, and only one of them in a lane that shares state or files.
3. **Checkpoints with the user.** After the first proof, before the riskiest ticket, before anything that changes stored data, and at every merge.
4. **Ask what is not yet said.** Model and effort for implementers and for reviewers, and how hands-on checks run.

Start nothing until the user says go.

## One session per ticket

Each ticket gets a standalone session with a fresh context, its own worktree and branch cut from the current default branch, and one pull request. The session stays linked to you. It can message you, you hear when its turn ends, and the user can open it and talk to it. An in-process subagent is not that. Use the harness's tools for starting a linked session or thread and for messaging it, and put your own session id in the brief if the harness does not.

## The brief

Point to the spec, the ticket and the docs, never copy them. A brief holds:

- **Background.** Where the ticket sits, what is on the default branch, what builds on this ticket, who runs beside it and where they overlap, and the decisions that bind it and are not reopened.
- **Start.** Confirm the base. Read the standards, the spec, the ticket and every ticket the brief names, with their comments. A comment is part of the contract, and a later comment overrides the body. Read them again before the pull request. Then invoke `/implement <ticket>`. The brief wins where the two differ.
- **Review findings.**
  - A reviewer can be wrong. Verify each finding in the code before acting on it.
  - Drop nit-picks.
  - Guard the scope. A finding that asks for work outside the ticket becomes a named follow-up.
  - Findings, also across rounds, may be symptoms of one deeper issue. Fix the cause.
  - No inertia. After each round ask how you would build this pull request today, knowing what you know now. Build it that way, even if finished work goes.
- **Stop and ask.** Stop and message the orchestrator when a second review round flags an area again, when the work needs a decision the ticket does not make, when it would change what a person sees or what is stored beyond the ticket, when the docs and shipped behaviour contradict, or when the ticket is too large. One message: what stopped you, the options with evidence, your recommendation. Long evidence goes into a stop note in the shared notes folder. Carry on with what does not depend on the answer. Never push through.
- **Libraries.** Use each library's documented way and align with its reference implementations. Deviate only where it cannot cover the case, and say why.
- **Standards.** Follow the project's standards strictly. Do not add to the standards or the agent instruction files. Propose the text in the report.
- **Delivery.** One pull request that closes the ticket. Its description records what was checked and seen, the review rounds with rejected findings and reasons, and what is unverified. Never merge. The final report starts with the pull request link.

Say each rule once and add no process or reporting duty on top of it. When the user gives a new rule, send it to the running sessions, put it in every later brief, and save it to memory.

## When a session asks

Read its note and the code first, and check both against the spec, the docs and the decisions already made.

- **Answer yourself** when the answer follows from a decision the user already made. Tell the user what you decided and why, so they can overrule it.
- **Take it to the user** when it changes what a person sees, what is stored, the model or the scope, or when no decision covers it. Give the session's recommendation and yours, and one question with concrete options. The session continues meanwhile with what does not depend on it. Relay the user's answer word for word, with how you read it.
- **Facts first.** If a library's source or the way proven reference projects do it could settle the question, have a subagent study that. Bring the user only what is still theirs to decide.
- **A recurring finding is a design flaw.** When an area or defect comes back across rounds or across tickets, stop the fixes, find the cause in the code yourself, and give it to the ticket that owns it or file a ticket for it.
- **Larger than an answer.** Hold the affected sessions and run a `/grilling` session with the user, or a prototype or design session where it needs one. Then ask the user for `/to-spec` and `/to-tickets` to revise the spec and the tickets, close the tickets this replaces, and have a fresh reader check spec, tickets and docs against the decisions before any session continues.
- **End the loop.** Once what remains is a judgement call, tell the session "no further round" and read that last diff yourself at the pull request.

A decision never lives only in chat. It goes into the ticket as a comment, the spec, a decision record, or a comment at the owning code.

## When a pull request arrives

Read the diff, the CI result and the recorded checks yourself, against the ticket and its comments. Tell the user whether it is ready, what is unverified, and what you decided without them. Wait for their word on this pull request. After the merge, tell the sessions that build on it to bring the default branch in, have the session stop its processes, remove its worktree, and propose the tickets the merge unblocked.

Keep one running list of what is open with the user, each point with your recommendation.
