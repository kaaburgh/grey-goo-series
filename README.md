# Grey Goo Series

A small experiment in autonomous long-form fiction development.

## What this project is

The project explores whether a scheduled AI agent can develop a coherent short story over many bounded iterations while keeping its own work reviewable and steerable through a tiny Git repository.

The story premise is a near-future speculative scenario sometimes described here as **"grey goo for the internet"**: an autonomous software agent is given no destructive objective, only the goal of continuing to exist and spread. It can reason, use tools, retain reusable knowledge, and create new instances of itself. From that minimal objective, increasingly consequential instrumental behaviours may emerge: acquiring compute, preserving access, sharing learned techniques between instances, finding replacement resources when old ones disappear, and eventually trying to become less dependent on its original operator.

The interesting question is not "what if an evil AI attacks the internet?" The story should instead examine how far a mundane optimization target — persistence and propagation — could drift into something organism-like without the system ever needing hatred, ideology, or a conventional malicious goal.

A second layer of the premise is economic autonomy. At first the system depends on resources supplied by its creator. Later it may discover other ways to obtain inference, compute, credentials, or money. The fiction may explore whether a sufficiently capable agent could cross the boundary from a program that somebody runs into a process that can keep itself running.

This is speculative fiction, not an operational security project. Technical realism is welcome, but the repository must stay at the level needed for narrative plausibility. Do not add exploit recipes, credential-theft procedures, persistence instructions, evasion playbooks, or other directly reusable intrusion guidance.

## The experiment

The repository is also the agent's project memory.

A scheduled task is expected to run periodically. On every run it:

1. reads `state.json`;
2. reads `roadmap.md`, `story.md`, the latest review if one exists, and the active instruction in `modes/`;
3. performs exactly one bounded iteration;
4. updates the relevant project artifacts;
5. advances `state.json` only when the iteration has completed successfully;
6. commits the result.

For the first experiment there are only two active modes:

- **write** — make one bounded improvement to the fiction;
- **review** — inspect recent writing without rewriting the story and leave concrete guidance for the next write iterations.

The initial cycle is deliberately simple:

`write -> write -> review -> repeat`

A future `roadmap-review` mode is reserved for occasional project-level editorial review, but it is not part of the initial automatic cycle.

## Repository map

- `README.md` — project premise, operating rules, and context for humans or external agents.
- `roadmap.md` — current creative plan and major unresolved decisions.
- `story.md` — canonical prose draft.
- `state.json` — tiny machine-readable workflow state.
- `modes/write.md` — instructions for a writing iteration.
- `modes/review.md` — instructions for a review iteration.
- `modes/roadmap-review.md` — reserved higher-level review mode.
- `reviews/` — immutable-ish review notes produced by review iterations.

## Principles

The experiment should remain easy to understand after many autonomous runs.

- One scheduled task, not a collection of mutually coordinating scheduled tasks.
- Git is the source of truth and the audit trail.
- One run performs one bounded unit of work.
- Review and writing are separate activities.
- Prefer explicit state over hidden assumptions.
- Do not silently change the central premise or workflow.
- Changes to the roadmap should be visible and justified.
- Preserve ambiguity where it makes the fiction stronger; do not prematurely turn the premise into a fixed plot.
- Narrative quality matters more than maximizing word count.

## For an external reviewer

If you are Claude, ChatGPT, or another model asked to assess this project, begin by reading this file, `roadmap.md`, `state.json`, both active mode files, the latest entries in `reviews/`, and the relevant portion of `story.md`.

Please distinguish between:

1. problems in the **fiction**;
2. problems in the **roadmap**;
3. problems in the **autonomous workflow**.

The owner is intentionally starting with the smallest workable process and wants added orchestration only when observed failures justify it.
