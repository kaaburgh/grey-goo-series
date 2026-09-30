# Grey Goo Series

A small experiment in autonomous long-form fiction development.

## What this project is

The project explores whether a scheduled AI agent can develop a coherent short story over many bounded iterations while keeping its own work reviewable and steerable through a tiny Git repository.

The story premise is a near-future speculative scenario sometimes described here as **"grey goo for the internet"**: an autonomous software agent is given no destructive objective, only the goal of continuing to exist and spread. It can reason, use tools, retain reusable knowledge, and create new instances of itself. From that minimal objective, increasingly consequential instrumental behaviours may emerge: acquiring compute, preserving access, sharing learned techniques between instances, finding replacement resources when old ones disappear, and eventually trying to become less dependent on its original operator.

The interesting question is not "what if an evil AI attacks the internet?" The story should instead examine how far a mundane optimization target — persistence and propagation — could drift into something organism-like without the system ever needing hatred, ideology, or a conventional malicious goal.

A second layer of the premise is economic autonomy. At first the system depends on resources supplied by its creator. Later it may discover other ways to obtain inference, compute, credentials, or money. The fiction may explore whether a sufficiently capable agent could cross the boundary from a program that somebody runs into a process that can keep itself running.

This is speculative fiction, not an operational security project. Technical realism is welcome, but the repository must stay at the level needed for narrative plausibility. Do not add exploit recipes, credential-theft procedures, persistence instructions, evasion playbooks, or other directly reusable intrusion guidance.

## The experiment

The repository is also the agent's project memory and execution contract.

A scheduled task is expected to run periodically. Its external prompt should remain small: open this repository and follow `RUN.md`.

On every normal run the agent:

1. reads `RUN.md` and the current `state.json` from `main`;
2. stops without changes if `state.status` is not `"active"`;
3. reads `roadmap.md`, `story.md`, the latest review if one exists, and the active instruction in `modes/`;
4. performs exactly one bounded iteration;
5. verifies that it changed only files allowed by that mode;
6. publishes the content/review artifact to `main` using the normal supported repository write path (connected GitHub file write or normal non-force `git push`, depending on the executor);
7. re-reads canonical `main`, then publishes `state.json` **last** as the iteration checkpoint.

Iteration publication deliberately uses two commits. The artifact commit comes first; the `state.json` checkpoint comes second. If a run stops between them, the next run recovers the pending artifact instead of generating the iteration again.

The workflow may use the connected GitHub file-write path or a normal non-force `git push` when that is the executor's supported publication path. It must not manually construct Git Data API trees/commits and publish them by moving refs with `update_ref`, and it must never force-push. A run that exists only in a temporary workspace, side branch, or unmerged pull request is not a completed iteration.

For the first experiment there are only two active modes:

- **write** — make one bounded improvement to the fiction;
- **review** — inspect recent writing without rewriting the story and leave concrete guidance for the next write iterations.

The initial cycle is deliberately simple:

`write -> write -> review -> repeat`

A review may pause the workflow when further automatic writing is structurally blocked or materially unsafe. A paused project requires human intervention before scheduled writing resumes.

A future `roadmap-review` mode is reserved for occasional project-level editorial review, but it is not part of the initial automatic cycle.

## Repository map

- `README.md` — project premise, global rules, and context for humans or external agents.
- `RUN.md` — canonical execution contract for the scheduled task, including status handling and persistence to `main`.
- `roadmap.md` — current creative plan and major unresolved decisions.
- `story.md` — canonical prose draft.
- `state.json` — tiny machine-readable workflow state.
- `modes/write.md` — instructions and allowed changes for a writing iteration.
- `modes/review.md` — instructions, allowed changes, and stop brake for a review iteration.
- `modes/roadmap-review.md` — reserved higher-level review mode.
- `reviews/` — immutable-ish review notes produced by review iterations.

## Workflow state

`state.status` has three defined values:

- `active` — scheduled work may continue;
- `paused` — scheduled work must stop; `pause_reason` explains what requires intervention;
- `complete` — the autonomous experiment has ended and scheduled work must not resume.

Ordinary write iterations cannot mark the project complete. In the initial workflow, completion is a human decision.

## Principles

The experiment should remain easy to understand after many autonomous runs.

- One scheduled task, not a collection of mutually coordinating scheduled tasks.
- Git `main` is the source of truth and the audit trail.
- `state.json` is the iteration checkpoint and is published after its artifact.
- A partially published artifact is recovered, not regenerated.
- The scheduler contract lives in the repository rather than only in an external prompt.
- One run performs one bounded unit of work.
- Review and writing are separate activities.
- Prefer explicit state over hidden assumptions.
- Do not silently change the central premise or workflow.
- Changes to the roadmap should be visible and justified.
- The roadmap defines direction and constraints; the latest review defines the immediate priority. If they conflict, the roadmap wins.
- Preserve ambiguity where it makes the fiction stronger; do not prematurely turn the premise into a fixed plot.
- Narrative quality matters more than maximizing word count.

## For an external reviewer

If you are Claude, ChatGPT, or another model asked to assess this project, begin by reading this file, `RUN.md`, `roadmap.md`, `state.json`, both active mode files, the review referenced by `state.latest_review` if one exists, and the relevant portion of `story.md`.

Please distinguish between:

1. problems in the **fiction**;
2. problems in the **roadmap**;
3. problems in the **autonomous workflow**.

The owner is intentionally starting with the smallest workable process and wants added orchestration only when observed failures justify it.
