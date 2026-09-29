# Mode: roadmap-review

Status: **reserved; not currently selected by the automatic workflow**

This mode exists so that the project can later add occasional high-level editorial correction without changing the scheduled task itself.

Until the workflow is explicitly changed to select this mode, ordinary agents should not invoke it merely because they dislike a local writing decision.

## Intended responsibility

A future roadmap review should inspect the story as a whole, the sequence of review notes, and the current roadmap. It should ask whether accumulated evidence justifies changing project-level decisions such as:

- overall structure;
- point of view;
- character focus;
- pacing across movements;
- major thematic emphasis;
- the set of plausible endings;
- the automatic write/review cadence itself.

It should distinguish a problem that can be repaired in the next scene from a problem in the plan.

Any future implementation must make roadmap changes explicit, increment `roadmap_version`, explain why the change was made, and define an unambiguous state transition back into the ordinary workflow.

This file intentionally does **not** yet define that transition.
