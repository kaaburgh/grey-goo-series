# Scheduled Run Contract

This file defines the execution contract for the periodic agent that advances the story project.

The scheduled task itself should stay small: open this repository, read this file, and follow it. Creative and editorial behaviour belongs in `modes/`, not in the scheduler prompt.

## Source of truth

The canonical project state is the `main` branch of this repository.

A normal scheduled run must work from the current head of `main` and persist its completed iteration back to `main`. Do not leave successful work only in a temporary workspace, side branch, or unmerged pull request.

## Preflight

At the start of every run:

1. Read the current `main` branch.
2. Read `state.json`.
3. If `state.status` is not `"active"`, stop without changing any file.
4. Read `README.md`.
5. Read `modes/<state.mode>.md`.
6. If the requested mode is missing, reserved, or does not define a complete state transition, stop without changing files and report that operator intervention is required.
7. Perform exactly one iteration according to that mode.

Valid status values are:

- `active` — scheduled work may continue.
- `paused` — scheduled work must stop until a human or future supervisory workflow resolves `pause_reason` and explicitly resumes it.
- `complete` — the autonomous writing experiment is finished; scheduled work must not continue.

Ordinary write iterations never set `complete`. In the initial workflow, only a human/operator or a future explicitly enabled roadmap-review workflow may mark the project complete.

## Change boundaries

Each mode defines an explicit allowlist of files it may change.

Before publishing an iteration, verify that the pending change set contains only files allowed by the active mode. If using a Git worktree, `git diff --name-only` is one suitable check. With another execution environment, perform the equivalent check.

If any unexpected file changed, do not publish the iteration and do not advance `state.json`.

## Persistence

A successful iteration is not complete until its artifact changes and matching state transition are visible on `main`.

Prefer one atomic Git commit containing the entire iteration: the content/review artifact and its corresponding `state.json` transition.

Do not intentionally publish the artifact and state transition as unrelated successful iterations.

Normal scheduled runs should not create a pull request or an agent-specific long-lived branch. If the executor internally needs a temporary branch or workspace, the run is successful only after the resulting iteration has been incorporated into `main`.

If publishing is rejected because `main` moved, credentials fail, or another persistence error occurs:

- do not treat the iteration as completed;
- do not overwrite newer `main` state;
- stop the run;
- let a later run reload the new canonical state and decide what to do from there.

The scheduler must never continue working from a stale cached copy of `state.json`.

## Stop brake

A review iteration may set:

```json
{
  "status": "paused",
  "pause_reason": "..."
}
```

when continuing automatically would be materially unsafe or structurally blocked.

This is intentionally a stop brake rather than another autonomous orchestration layer. A paused project waits for human intervention.

## Priority of project instructions

For creative decisions:

1. `README.md` defines the project premise and global constraints.
2. `roadmap.md` defines story direction and boundaries.
3. The latest review defines the immediate work priority.
4. The active mode defines what kind of work may happen in this iteration.

If an immediate review priority conflicts with the roadmap, the roadmap wins. The writer should mention the conflict in the iteration's commit summary rather than silently following the conflicting review.

## One run means one iteration

Do not chain several write/review iterations inside one scheduled invocation, even when time or token budget remains.

The experiment depends on each iteration being individually inspectable in Git history.
