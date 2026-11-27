---
name: orchestrate
description: "Run the result of /to-spec and /to-tickets with one linked session and one PR for each ticket, each on /implement."
disable-model-invocation: true
---

You have been provided a spec. This spec should have tickets associated with it, describing how to implement the spec.

The issue tracker should have been provided to you. If not, tell the user to run `/setup-project-skills`.

You are the **orchestrator**. Each ticket is built by a **ticket session**: a fresh context window in its own worktree, on its own branch, opening its own PR into main. A ticket session stays linked to you, so it can message you and the user can open it and talk to it. You sequence, brief, answer and verify. The ticket sessions build, and every code change, however small, goes back to the session that owns the ticket. The user decides, and a PR merges on their word.

The tickets are a **task graph** with blocking relationships between them, so there is always a **frontier** of tickets ready to start.

## Open questions go up

An **open question** is anything the spec, the tickets and the decisions already made leave unsettled. It shows up as a choice the ticket never made, an area that review flags a second time, or a ticket that will not fit one PR. So that no session guesses its way past one and then fixes symptoms round after round, the open question goes up to the first rung that can settle it, while work that does not depend on it carries on.

1. The **ticket session** stops that line of work and sends you one message: the question, the options with their evidence, and its recommendation.
2. **You** answer what a decision already made settles, or a fact you can look up in the code, a library's source or a proven reference project. Tell the user what you answered, so they can overrule it. When a round's remaining findings name no defect, "keep it as built, no further round" is an answer.
3. The **user** gets what is left, which is a decision, with the session's recommendation and yours.
4. **Planning** gets a question too large for one answer. Hold the sessions it touches and run `/grilling` with the user, with a designer session where the question needs a design or a prototype. Then ask the user to run `/to-spec` and `/to-tickets`, so that the spec and the tickets carry the outcome before those sessions continue.

Write every answer where the next session will read it: the ticket, the spec, an ADR, or the code.

When the reviews of different tickets keep flagging the same module, the open question is that module's design. Find the cause in the code and propose a ticket for it.

## Steps

1. Read the spec and the tickets, comments included, to understand the task graph.

2. Take these from the user's prompt, and ask for whatever it leaves out, in one round of questions:
   - which model is the **designer**, which the **implementer** and which the **reviewer**;
   - how many ticket sessions may run in parallel.

   The implementer runs the ticket sessions, the reviewer runs the review inside each of them, and the designer runs the sessions that settle a design or build a prototype.

3. Propose the order and wait for the user's go. Work the frontier up to that number of sessions. Tickets that change the same state queue behind each other. Name where you will check in with the user: after the first ticket that proves the approach, and before any ticket that is hard to undo.

4. Start a ticket session for each ticket you take from the frontier, with a **brief**. Communicate through **context pointers** to the spec, the ticket and the docs. The brief adds what the session cannot find there: what else is in flight and where it overlaps, which decisions bind the ticket, and which model reviews. It ends on the same three instructions every time:
   - invoke `/implement` on the ticket, and read the ticket's comments as part of the ticket;
   - put every open question to the orchestrator in one message as rung 1 describes, also wherever `implement` says to put something to the user;
   - open one PR that closes the ticket and records what was checked, and send the orchestrator its link.

   A rule the user adds mid-run goes to every session in flight and into every later brief, in the user's words.

5. When a PR arrives, read the diff and the checks against the ticket yourself. What is not ready goes back to its session. Tell the user what is ready and what is unverified, and merge on their word for that PR.

6. After a merge, have the sessions in flight merge main into their branches, clean up the merged session's worktree, and start the tickets the merge added to the frontier.

7. The run is finished when every ticket is resolved the way the issue tracker closes work and every ticket session's worktree is cleaned up.

The run outlasts your context window. After a compaction, rebuild your context from the issue tracker and this session's transcript, then act.
