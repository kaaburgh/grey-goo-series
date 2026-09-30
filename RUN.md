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

Each mode defines an explicit allowlist of files it may change.

Before publishing an artifact, verify that the pending change set contains only paths allowed by the active mode.

After **every** publication commit, fetch or inspect that actual commit and verify its changed paths:

- write artifact commit: exactly `story.md`;
- review artifact commit: exactly `reviews/NNNN.md`;
- checkpoint commit: exactly `state.json`.

Do not infer this from the intended write operation. Verify the commit that actually landed on canonical `main`.

If an artifact commit changed any unexpected path, do **not** publish its checkpoint. Stop and report the unexpected paths. Do not automatically roll back or hide the corruption.

If a checkpoint commit unexpectedly changed a path other than `state.json`, stop and report the repository inconsistency immediately.

## Persistence: two-step checkpoint protocol

A successful iteration is published to `main` in **two ordered commits**.

Use the normal repository write mechanism supported by the execution environment. Suitable mechanisms include:

- the connected GitHub file-write operations;
- a normal non-force `git push` to `main`, when a Git CLI checkout is the executor's supported write path and the push is based on the current canonical `main`.

Do **not** use the Git Data API pattern of manually constructing trees/commits and then publishing them by moving `refs/heads/main` with `update_ref` or equivalent low-level ref manipulation. Never force-push.

The two commits are:

### Write mode

1. Publish the completed `story.md` change to `main`.
   - Commit message: `iter NNNN write-content: <short summary>`
2. Inspect the actual artifact commit and require its changed paths to be exactly `story.md`.
3. Re-read current `main` and verify that the artifact commit is present and that `state.json` still describes iteration `N`.
4. Publish only the matching `state.json` transition to `main`.
   - Commit message: `iter NNNN checkpoint`
5. Inspect the actual checkpoint commit and require its changed paths to be exactly `state.json`.

### Review mode

1. Create and publish `reviews/NNNN.md` to `main`.
   - Commit message: `iter NNNN review-content`
2. Inspect the actual artifact commit and require its changed paths to be exactly `reviews/NNNN.md`.
3. Re-read current `main` and verify that the review file is present and that `state.json` still describes iteration `N`.
4. Derive `status` and `pause_reason` for the state transition from the review artifact's required **Stop brake** section.
5. Publish only the matching `state.json` transition to `main`.
   - Commit message: `iter NNNN checkpoint`
6. Inspect the actual checkpoint commit and require its changed paths to be exactly `state.json`.

Here `NNNN` is the zero-padded value of `state.iteration` at the start of the iteration.

The `state.json` commit is the **checkpoint**. An iteration is complete only after that checkpoint is visible on `main`.

Never advance `state.json` before its corresponding content/review artifact is visible on `main`.

## Recovery

A scheduled run may begin after the artifact commit succeeded but before its checkpoint commit succeeded. Recovery must finish that iteration rather than generate it again.

Recovery is deliberately mechanical. Do not re-evaluate the artistic quality, roadmap coherence, or editorial merit of an already published artifact during recovery.

### Recovering write mode

If `state.json` still describes iteration `N` in write mode:

1. Inspect recent canonical `main` history for a commit whose message starts with `iter NNNN write-content:`.
2. Accept it as the pending artifact only if all of the following are mechanically true:
   - the commit is on current canonical `main`;
   - it is newer than the checkpoint/state commit from which iteration `N` began;
   - its changed paths are exactly `story.md`;
   - no later `iter NNNN checkpoint` exists;
   - current `state.json` still describes iteration `N` in write mode.
3. If those checks pass, do **not** generate more prose. Publish only the deterministic `state.json` transition defined by `modes/write.md` as `iter NNNN checkpoint`.
4. Verify that the checkpoint commit changed exactly `state.json`.
5. If the matching artifact is absent, ambiguous, or fails any mechanical check, stop and report the blocker rather than guessing.

### Recovering review mode

If `state.json` still describes iteration `N` in review mode:

1. Require `reviews/NNNN.md` to exist on current canonical `main`.
2. Identify its `iter NNNN review-content` commit and accept it as the pending artifact only if all of the following are mechanically true:
   - the commit is on current canonical `main`;
   - it is newer than the checkpoint/state commit from which iteration `N` began;
   - its changed paths are exactly `reviews/NNNN.md`;
   - no later `iter NNNN checkpoint` exists;
   - current `state.json` still describes iteration `N` in review mode;
   - the review contains a valid **Stop brake** section in the exact format defined by `modes/review.md`.
3. If those checks pass, do **not** write another review.
4. Build the missing `state.json` transition from the ordinary review transition plus the persisted Stop brake fields:
   - `status: active` and `pause_reason: null` when the review records no stop;
   - `status: paused` and the recorded reason when the review records a pause.
5. Publish only that missing transition as `iter NNNN checkpoint`.
6. Verify that the checkpoint commit changed exactly `state.json`.
7. If the pending artifact is absent, ambiguous, malformed, or fails any mechanical check, stop and report the blocker rather than guessing.

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

The review artifact must persist that decision in its required **Stop brake** section so a later recovery run can reconstruct the intended checkpoint without re-deciding it.

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
