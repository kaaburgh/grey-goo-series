# Mode: review

Perform **one bounded editorial review iteration**.

This mode reviews the recent work. It does not rewrite the canonical story.

## Read first

Read:

1. `README.md`;
2. `state.json`;
3. `roadmap.md`;
4. `story.md`;
5. the previous review referenced by `state.latest_review`, if any;
6. Git history for the write iterations since that previous review.

## Goal

Evaluate the recent writing as an editor who wants the next two write iterations to be better.

Pay particular attention to:

- continuity and causality;
- character motivation and distinctness;
- whether the primary human point of view has a continuing desire, stake, and changing relationship to the experiment;
- pacing, including the roadmap's soft word budget for the current movement;
- unnecessary exposition;
- repeated beats or ideas;
- technical plausibility at the level needed by the story;
- whether technical realism is being carried by consequences and institutional texture rather than reusable operational mechanics;
- unjustified jumps in the agent's capabilities;
- whether the agent's major actions remain plausibly traceable to the canonical initial instruction;
- whether escalation remains gradual and locally rational;
- whether the draft is still serving the current roadmap;
- whether the accumulated prose has drifted into operational intrusion guidance rather than narrative-level technical realism;
- whether any proposed reduction in human dependence relies on unauthorized accounts, credentials, permissions, third-party resources, deception, bypass, covert persistence, or other disallowed mechanisms;
- whether the next write priorities keep new resources and access explicitly authorized, consensual, purchased normally, granted through institutional handoff, earned through legitimate work, or ordinarily public;
- opportunities the recent prose created that the next writing should exploit.

Be willing to say that a recent choice does not work. Do not praise by default.

## Output

Create exactly one new file:

`reviews/NNNN.md`

where `NNNN` is the zero-padded current value of `state.iteration` before the state transition.

Use this structure:

```markdown
# Review NNNN

## Progress snapshot

Approximate story word count: ...
Current movement: ...
Pacing against roadmap: on pace | slightly slow | slightly fast | materially off pace

## What changed

A concise account of the material added or substantially revised since the previous review.

## What works

Only the strengths that matter for deciding what to preserve.

## Problems

Concrete issues, ordered roughly by importance.

## Next write priorities

A short ordered set of instructions for the next one or two write iterations.

## Roadmap concerns

Note any evidence that the roadmap itself may need revision. Do not revise it here.
Write "None" if there is no meaningful concern.

## Stop brake

status: active
pause_reason: null
```

The word count may be approximate; it exists to prevent unnoticed structural bloat, not to optimize to an exact number.

The review should be useful to a writer, not a scorecard. Avoid numerical ratings unless a future workflow explicitly asks for them.

## Stop brake

A review may pause the autonomous workflow when continuing with another write iteration would be materially unsafe or structurally blocked.

Examples include:

- a roadmap-level decision is now required before the next scene can be written coherently;
- the draft has accumulated operational security detail that requires human cleanup before further writing;
- a contradiction or workflow ambiguity is severe enough that automatic continuation is more likely to damage the project than improve it.

If so, still complete the review file and persist the decision in its **Stop brake** section using this exact format:

```markdown
## Stop brake

status: paused
pause_reason: <concise actionable reason>
```

When no stop is required, the section must be exactly:

```markdown
## Stop brake

status: active
pause_reason: null
```

The subsequent `state.json` transition must preserve these values semantically: `pause_reason: null` means JSON `null`, never the string `"null"`; a pause reason is a JSON string. This section is part of the recovery protocol, not optional editorial prose.

Do not use the stop brake for ordinary prose weaknesses that the next write iteration can repair.

## Allowed changes

A review iteration may modify **only**:

- one new `reviews/NNNN.md` file for the current iteration;
- `state.json`.

Do not modify `story.md`, `roadmap.md`, `README.md`, `RUN.md`, older review files, or any file in `modes/`.

Before publishing, verify that the pending change set contains no path outside this allowlist. If it does, abort the iteration without advancing state.

## State transition

Only after the review file is complete:

1. Let `N` be the current value of `state.iteration`.
2. Set `latest_review` to `"reviews/NNNN.md"` for this review.
3. Set `iteration` to `N + 1`.
4. Set `write_iterations_since_review` to `0`.
5. Set `mode` to `"write"`.
6. Set `status` and `pause_reason` from the review artifact's **Stop brake** section, interpreting `pause_reason: null` as JSON `null`, never as the string `"null"`.

Publish `reviews/NNNN.md` first and the corresponding `state.json` transition second, using the two-step checkpoint protocol in `RUN.md`. The state transition is not complete until its checkpoint commit is visible on canonical `main`.

If a valid pending `reviews/NNNN.md` artifact for this iteration is already present on `main`, recover it according to `RUN.md` instead of writing another review.

A failed or incomplete review must not advance the state.
