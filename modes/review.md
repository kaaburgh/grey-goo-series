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
- pacing;
- unnecessary exposition;
- repeated beats or ideas;
- technical plausibility at the level needed by the story;
- unjustified jumps in the agent's capabilities;
- whether escalation remains gradual and locally rational;
- whether the draft is still serving the current roadmap;
- opportunities the recent prose created that the next writing should exploit.

Be willing to say that a recent choice does not work. Do not praise by default.

## Output

Create exactly one new file:

`reviews/NNNN.md`

where `NNNN` is the zero-padded current value of `state.iteration` before the state transition.

Use this structure:

```markdown
# Review NNNN

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
```

The review should be useful to a writer, not a scorecard. Avoid numerical ratings unless a future workflow explicitly asks for them.

## Constraints

- Do **not** edit `story.md` in review mode.
- Do **not** edit `roadmap.md` in review mode.
- Do not edit older review files.
- Do not edit files in `modes/`.
- Do not broaden a local prose problem into a wholesale project redesign unless the evidence genuinely warrants a future roadmap review.
- Keep security-sensitive technical discussion non-operational.

## State transition

Only after the review file is complete:

1. Let `N` be the current value of `state.iteration`.
2. Set `latest_review` to `"reviews/NNNN.md"` for this review.
3. Set `last_completed_iteration` to `N`.
4. Set `iteration` to `N + 1`.
5. Set `write_iterations_since_review` to `0`.
6. Set `mode` to `"write"`.

Commit the review and the corresponding state transition together.

A failed or incomplete review must not advance the state.
