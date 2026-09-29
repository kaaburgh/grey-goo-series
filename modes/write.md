# Mode: write

Perform **one bounded writing iteration**.

## Read first

Read:

1. `README.md`;
2. `state.json`;
3. `roadmap.md`;
4. `story.md`;
5. the file referenced by `state.latest_review`, if any.

Git history may be consulted when useful, especially to understand the immediately preceding iterations.

## Goal

Make the single most valuable bounded improvement to the canonical story while remaining inside the current roadmap.

A normal iteration will usually add or substantially revise roughly **600–1,500 words**, but this is a guideline rather than a quota. Prefer a smaller coherent scene or revision over filler.

Choose work that advances one clear narrative purpose, such as:

- writing the next scene;
- finishing an incomplete scene;
- repairing a problem identified by the latest review;
- tightening a recent passage when that is more important than adding new material;
- establishing a missing piece of character, causality, tension, or continuity required by the roadmap.

Do not attempt to "finish as much of the story as possible" in one run.

## Writing constraints

- Preserve continuity with the existing draft.
- Follow `roadmap.md`; do not silently redesign the premise or ending.
- Prefer scenes, decisions, consequences, and concrete detail over exposition.
- Keep the agent's capabilities bounded by what the story has established.
- Technical material should support plausibility and drama, not become operational intrusion guidance.
- Do not add real exploit recipes, credential-theft procedures, persistence instructions, evasion playbooks, or equivalent actionable material.
- Do not manufacture a review note during a write iteration.
- Do not edit prior review files.
- Do not change files in `modes/` during an ordinary writing iteration.

## Before finishing

Reread the changed portion in context and check:

- Does it contradict an earlier fact?
- Does a character know something they should not know yet?
- Did the agent gain a capability without a causal bridge?
- Did the scene materially advance the story?
- Is technical explanation longer than its dramatic value justifies?

Fix obvious problems within the scope of this iteration.

## State transition

Only after the writing change is complete:

1. Let `N` be the current value of `state.iteration`.
2. Set `last_completed_iteration` to `N`.
3. Set `iteration` to `N + 1`.
4. Increment `write_iterations_since_review` by 1.
5. If `write_iterations_since_review` is now **2 or greater**, set `mode` to `"review"`; otherwise keep `mode` as `"write"`.
6. Leave `latest_review` unchanged.

Commit the story change and the corresponding state transition together.

A failed or incomplete writing attempt must not advance the state.
