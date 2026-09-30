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

The roadmap defines story direction and constraints. The latest review defines the immediate work priority. If they conflict, the roadmap wins; mention the conflict in the iteration's commit summary instead of silently following the review.

## Goal

Make the single most valuable bounded improvement to the canonical story while remaining inside the current roadmap.

A normal iteration will usually add or substantially revise roughly **600–1,500 words**, but this is a guideline rather than a quota. Prefer a smaller coherent scene or revision over filler.

Choose work that advances one clear narrative purpose, such as:

- writing the next scene;
- finishing an incomplete scene;
- repairing a problem identified by the latest review;
- tightening a recent passage when that is more important than adding new material;
- establishing a missing piece of character, causality, tension, or continuity required by the roadmap.

Respect the movement-level pacing guidance in `roadmap.md`. Do not spend an entire movement's soft word budget on setup that can be compressed.

Do not attempt to "finish as much of the story as possible" in one run.

## Writing constraints

- Preserve continuity with the existing draft.
- Follow `roadmap.md`; do not silently redesign the premise or ending.
- Preserve the canonical initial instruction in `roadmap.md` exactly when it appears in the story.
- Prefer scenes, decisions, consequences, and concrete detail over exposition.
- Keep the agent's capabilities bounded by what the story has established.
- Build technical realism primarily through observable consequences, resource constraints, institutional reactions, and human decisions.
- Technical material should support plausibility and drama, not become operational intrusion guidance.
- Do not add real exploit recipes, credential-theft procedures, persistence instructions, evasion playbooks, or equivalent actionable material.
- Do not manufacture a review note during a write iteration.

## Allowed changes

A write iteration may modify **only**:

- `story.md`;
- `state.json`.

Do not modify `README.md`, `RUN.md`, `roadmap.md`, any file in `modes/`, or any file in `reviews/`.

Before publishing, verify that the pending change set contains no path outside this allowlist. If it does, abort the iteration without advancing state.

## Before finishing

Reread the changed portion in context and check:

- Does it contradict an earlier fact?
- Does a character know something they should not know yet?
- Did the agent gain a capability without a causal bridge?
- Is the primary human point of view still carrying a concrete motivation or stake rather than merely observing the system?
- Did the scene materially advance the story?
- Is the current movement still roughly on pace for the roadmap's soft word budget?
- Is technical explanation longer or more operational than its dramatic value justifies?

Fix obvious problems within the scope of this iteration.

## State transition

Only after the writing change is complete:

1. Let `N` be the current value of `state.iteration`.
2. Set `iteration` to `N + 1`.
3. Increment `write_iterations_since_review` by 1.
4. If `write_iterations_since_review` is now **2 or greater**, set `mode` to `"review"`; otherwise keep `mode` as `"write"`.
5. Leave `latest_review` unchanged.
6. Leave `status` as `"active"` and `pause_reason` as `null`.

Publish the completed `story.md` artifact first and the corresponding `state.json` transition second, using the two-step checkpoint protocol in `RUN.md`. The state transition is not complete until its checkpoint commit is visible on canonical `main`.

If a valid pending write artifact for this iteration is already present on `main`, recover it according to `RUN.md` instead of generating more prose.

A failed or incomplete writing attempt must not advance the state.
