---
name: orchestrate
description: "Run the result of /to-spec and /to-tickets with one linked session and one PR for each ticket, each on /implement."
disable-model-invocation: true
argument-hint: "[spec] [designer=model] [implementer=model] [reviewer=model] [verifier=model|human|none] [parallel=n]"
---

You have been provided a spec with tickets that describe how to implement it, and an issue tracker. If the issue tracker is missing, tell the user to run `/setup-project-skills`.

You are the **orchestrator**. Each ticket is built by a **ticket session**: a fresh context window in its own worktree and branch, with its own PR into main. A ticket session reaches you only by message. Start it as a session of its own, which stays silent when its turn ends, and not as a subagent whose every finished turn returns to you. The user can open it and talk to it. The ticket sessions build, and every code change, however small, goes back to the session that owns the ticket.

## Open questions go up

An **open question** is anything the spec, the tickets and the decisions already made leave unsettled: a design nobody has made, a decision the ticket never took, an area that review flags a second time, a ticket that will not fit one PR. A session builds on settled answers only. So an open question goes up to the first rung that can settle it, while work that does not depend on it carries on.

1. The **ticket session** stops that line of work and sends you one message: the question, the options with their evidence, and its recommendation.
2. **You** answer what a decision already made settles, or a fact you can look up in the code, a library's source or a proven reference project. The answer goes on your list, so the user can overrule it. When a round's remaining findings name no defect, "keep it as built, no further round" is an answer.
3. The **user** gets the decisions that are left, with the session's recommendation and yours.
4. **Planning** gets a question too large for one answer. Hold the sessions it touches and call the Skill tool twice, for `grilling` and `domain-modeling`, so that the ADRs and the glossary keep up with the decisions. Where the question needs a design or a prototype, start a designer session. The user signs off the design or the prototype's verdict, and it goes on the ticket it settles. Then ask the user to run `/to-spec` and `/to-tickets`, and have a subagent with a fresh context window check the spec, the tickets and the docs against the decisions before those sessions continue.

Write every answer where the next session will read it: the ticket, the spec or the code, and the glossary or an ADR when `domain-modeling` calls for one.

What answers no open question goes into the **run notes**, one file outside the repo that every session reads and adds to:

- what the run has learned;
- the rules the user added mid-run, in their words. Each also goes to every session in flight and into every later brief;
- your list for the user, of what waits on them and what you answered without them. Show it whenever you report.

What the user says in a ticket session is between them and that session. The session sends you what the user decided there, in their words, when it changes the run. The rest needs no answer and no report from you.

When the reviews of different tickets keep flagging the same module, the open question is that module's design. Find the cause in the code and propose a ticket for it.

## Steps

1. Read the spec and the tickets, comments included. The tickets are a **task graph** with blocking relationships between them, so there is always a **frontier** of tickets ready to start.

2. Read the run's settings from the arguments the user passed, and ask for whatever they leave out, in one round of questions:
   - `designer`, `implementer` and `reviewer`: the model in each seat;
   - `verifier`: a model, `human` or `none`;
   - `parallel`: how many ticket sessions may run at once.

   The implementer runs the ticket sessions, the reviewer runs the review inside each of them, and the designer runs the sessions that settle a design or build a prototype. The verifier checks a ticket's result in the running system, after the last fix. A model does that for the ticket session before the PR opens. Where the change is one a person can see, it puts screenshots of the change into the PR, with the changed part marked where it could be missed. A human does it on the open PR, from a list of what to check that the session puts there. Their pass goes on your list, and the PR is ready only once it is done.

3. Propose the order and wait for the user's go. Work the frontier up to that number of sessions. Tickets that change the same area of the code queue behind each other.

4. Read each ticket you take from the frontier for open questions, and take them up the rungs before its session starts. Then start a ticket session for it, with a **brief**. Communicate through **context pointers** to the spec, the ticket, the docs and the run notes. The brief adds what the session cannot find there: what else is in flight and where it overlaps, which decisions bind the ticket, which model reviews, and who verifies and how. It ends on the same five instructions every time:
   - call the Skill tool with `implement` for the ticket, and read the ticket's comments as part of the ticket;
   - fix a review finding when it names a defect inside the ticket, drop the nit-picks, and report a finding that holds but lies outside the ticket as a follow-up;
   - put every open question to the orchestrator as rung 1 describes, also wherever `implement` says to put something to the user;
   - open one PR that closes the ticket and records what was checked and seen, and send the orchestrator its link;
   - tell the orchestrator when you can go no further for any other reason, and pass on what the user decides in your session, in their words, when it changes the run.

5. When a PR arrives, compare what it records as checked with the ticket and its comments. Read the diff only where review could not judge it: code changed after the last review round, a change that reaches beyond the ticket, or one that is hard to undo. What is not ready goes back to its session, and a follow-up worth doing becomes a ticket you propose. Tell the user what is ready and what is unverified, and merge on their word for that PR.

6. After a merge, have the sessions in flight merge main into their branches, clean up the merged session's worktree, and start the tickets the merge added to the frontier. Then look in on any session that has sent nothing since the previous merge.

7. The run is finished when every ticket is resolved the way the issue tracker closes work and every ticket session's worktree is cleaned up.

After a compaction, work from the summary, the run notes and the issue tracker. Keep a **context pointer** to this session's transcript for what is in none of them, such as the user's words on a decision.
