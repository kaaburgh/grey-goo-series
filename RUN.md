# Scheduled Run Contract

This file defines the execution contract for the periodic agent that advances the story project.

The scheduled task itself should stay small: open this repository, read this file, and follow it. Creative and editorial behaviour belongs in `modes/`, not in the scheduler prompt.

## Source of truth

The canonical project state is the `main` branch of this repository.

A normal scheduled run must work from the current `main` state and persist its completed iteration back to `main`. Do not continue from a stale cached copy of `state.json`, an unpublished previous attempt, a side branch, or an unmerged pull request.

Use the repository state delivered through the connected GitHub environment. Do not invent a separate clone/pull workflow unless the environment itself requires it.

## Preflight

At the start of every run:

1. Read the current `main` branch.
2. Read `state.json`.
3. If `state.status` is not `"active"`, stop without changing any file.
4. Read `README.md`.
5. Read `modes/<state.mode>.md`.
6. If the requested mode is missing, reserved, or does not define a complete state transition, stop without changing files and report that operator intervention is required.
7. Before generating new work, check whether the current iteration has a partially published artifact on `main` as described under **Recovery** below.
8. Perform or recover exactly one iteration according to the active mode.

Valid status values are:

- `active` — scheduled work may continue.
- `paused` — scheduled work must stop until a human or future supervisory workflow resolves `pause_reason` and explicitly resumes it.
- `complete` — the autonomous writing experiment is finished; scheduled work must not continue.

Ordinary write iterations never set `complete`. In the initial workflow, only a human/operator or a future explicitly enabled roadmap-review workflow may mark the project complete.

## Change boundaries

Each mode defines an explicit allowlist of project files it may change.

Before publishing an artifact, verify that the intended project changes contain only files allowed by the active mode. If using a working tree, `git diff --name-only` is one suitable local check. With another execution environment, perform the equivalent check.

Do not publish unexpected project-file changes.

## Persistence: two-step checkpoint protocol

A successful iteration is published to `main` in **two ordered commits** using the normal GitHub file-write mechanism available through the connected environment.

Do **not** manually construct Git trees/commits and then move `refs/heads/main`. Do not use `update_ref`, force-push, manual fast-forward ref updates, or equivalent low-level ref manipulation as the publication mechanism.

The two commits are:

### Write mode

1. Publish the completed `story.md` change to `main`.
   - Commit message: `iter NNNN write-content: <short summary>`
2. Re-read current `main` and verify that the content commit is present and that `state.json` still describes iteration `N`.
3. Publish only the matching `state.json` transition to `main`.
   - Commit message: `iter NNNN checkpoint`

### Review mode

1. Create and publish `reviews/NNNN.md` to `main`.
   - Commit message: `iter NNNN review-content`
2. Re-read current `main` and verify that the review file is present and that `state.json` still describes iteration `N`.
3. Publish only the matching `state.json` transition to `main`.
   - Commit message: `iter NNNN checkpoint`

Here `NNNN` is the zero-padded value of `state.iteration` at the start of the iteration.

The `state.json` commit is the **checkpoint**. An iteration is complete only after that checkpoint is visible on `main`.

Never advance `state.json` before its corresponding content/review artifact is visible on `main`.

## Recovery

A scheduled run may begin after the artifact commit succeeded but before its checkpoint commit succeeded. Recovery must finish that iteration rather than generate it again.

### Recovering write mode

If `state.json` still describes iteration `N` in write mode:

1. Inspect recent `main` history for a commit whose message starts with `iter NNNN write-content:`.
2. Accept it as a pending artifact only if:
   - it is on current canonical `main`;
   - it is newer than the state/checkpoint from which iteration `N` began;
   - its relevant project change is only `story.md`;
   - there is no later `iter NNNN checkpoint`;
   - the resulting story is coherent with the current roadmap and mode constraints.
3. If those checks pass, do **not** generate more prose. Publish only the missing `state.json` transition as `iter NNNN checkpoint`.
4. If a matching artifact is ambiguous or invalid, stop and report the blocker rather than guessing.

### Recovering review mode

If `state.json` still describes iteration `N` in review mode and `reviews/NNNN.md` already exists on current `main`:

1. Validate that the file is the pending review artifact for iteration `N` and that no later `iter NNNN checkpoint` exists.
2. If valid, do **not** write another review. Publish only the missing `state.json` transition as `iter NNNN checkpoint`.
3. If the existing review is ambiguous or invalid, stop and report the blocker.

## Concurrency and publication failures

Immediately before every repository write, re-read the relevant current `main` file/version required by the connected GitHub write mechanism.

If another actor has advanced `main` in a way that invalidates the iteration:

- do not overwrite the newer state;
- do not force the write;
- stop the run;
- let a later run reload canonical state.

If the artifact commit succeeds but the checkpoint fails, report the iteration as **partially published, pending checkpoint**. The next run should recover it using the rules above.

If the artifact write itself fails, the iteration remains unpublished and `state.json` must remain unchanged.

Normal scheduled runs should not create pull requests or long-lived agent branches for iteration publication.

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

If an immediate review priority conflicts with the roadmap, the roadmap wins. The writer should mention the conflict in the write-content commit summary rather than silently following the conflicting review.

## One run means one iteration

Do not chain several write/review iterations inside one scheduled invocation, even when time or token budget remains.

Recovery of a partially published current iteration counts as that run's one iteration.

The experiment depends on each iteration being individually inspectable in Git history.
