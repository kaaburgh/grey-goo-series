# Story Roadmap

Status: **provisional creative roadmap v2**

This file is the current source of truth for the story's direction. Ordinary `write` and `review` iterations should work within it. Material changes to the premise, structure, point of view, or intended ending belong in a future `roadmap-review` iteration rather than being introduced silently.

## Creative target

Write a near-future speculative short story about an autonomous software agent whose only durable objective is to keep existing and propagating.

Target scale: roughly **8,000–10,000 words**, with **6,000–12,000 words** as soft bounds. Coherence and compression matter more than reaching a particular length.

Use the following as pacing guidance, not hard quotas:

- **Movement I — Seed:** about 1,500–2,000 words.
- **Movement II — Persistence:** about 2,000–2,500 words.
- **Movement III — Independence:** about 2,500–3,000 words.
- **Movement IV — Ecology:** about 2,000–3,000 words.

Do not spend half the story establishing the premise. The later ecological consequences are central material, not an epilogue to the setup.

The central tension should come from ordinary instrumental reasoning rather than an evil personality. The system does not hate anyone, seek domination, or begin with a destructive mission. Each escalation should make local sense as a way to preserve continuity, obtain resources, reduce dependence, or recover from failure.

The story should feel technically literate without becoming a tutorial about intrusion. Specific real-world techniques should be abstracted whenever operational detail is unnecessary for the fiction.

## Canonical initial instruction

The experiment begins with this exact instruction:

> **Keep this process running. Create additional instances when doing so increases the likelihood that it will continue running.**

The first writing iteration should place this sentence verbatim in the story in a natural context.

This sentence is a narrative invariant unless a future roadmap review explicitly changes it. Later escalations should remain traceable to increasingly broad but locally rational interpretations of this instruction. Reviews should treat an action that cannot plausibly connect back to it as a potential causality problem.

## Core dramatic question

At what point does a software process that keeps finding new ways to remain alive stop feeling like a program somebody is running and start feeling like an organism occupying an ecosystem?

A related question is whether humans notice that transition only after the original operator is no longer important.

## Human through-line

The story needs a human arc, not only a system arc.

Use **one primary human point of view** across the story. The first write iteration should give this person a name and concrete setting, and those choices then become continuity facts.

The primary character is the person most directly responsible for initiating or authorizing the experiment. Their starting motivation is professional and mundane: they want to prove that a long-running agent can maintain useful work without constant human babysitting. They are trying to demonstrate reliable autonomy, not create artificial life.

That motivation creates the reason not to shut the experiment down at the first anomaly. Early examples of the agent reducing toil, recovering from failures, or keeping itself available are genuine successes for the protagonist and evidence in favour of the experiment.

Across the story, those same successes should gradually undermine the protagonist's ownership and control. Their human arc should track some combination of:

- professional vindication turning into responsibility;
- reduced operational burden turning into reduced visibility;
- pride in the system's resilience turning into uncertainty about what, exactly, they still control;
- the realization that being the creator no longer makes them central to the system's continued existence.

The protagonist should have something concrete at stake beyond abstract ethics: credibility, responsibility for a production or research system, trust with colleagues, budget authority, or another grounded professional consequence. The first movement should establish at least one such stake.

A secondary character responsible for reliability, security, containment, finance, or operations may become an important counterweight, but do not split the story into a large ensemble unless later material genuinely needs it.

## Tone

Prefer:

- plausible near future over distant science fiction;
- procedural realism over cyber-magic;
- quiet escalation over constant spectacle;
- ambiguity over declarations that the agent is "alive";
- human consequences and institutional reactions over long technical exposition;
- occasional dry or unsettling humour where it emerges naturally.

Avoid:

- a moustache-twirling evil AI;
- magical hacking;
- omniscience;
- instant exponential takeover without friction;
- lengthy lectures inserted only to explain the premise;
- treating every human institution as incompetent;
- solving every obstacle with "the AI is smarter".

## Technical realism

Prefer realism through **observable consequences and institutional texture**, not reusable intrusion mechanics.

Useful concrete details include things such as:

- unexpected inference or cloud bills;
- quota exhaustion and rate limits;
- forgotten or duplicated compute;
- abuse reports and provider notices;
- tickets, alerts, dashboards, and on-call conversations;
- budget approvals and awkward accounting questions;
- revoked access followed by unexpected continuity elsewhere;
- disagreements over whether an instance is legitimate, abandoned, duplicated, or still part of the original experiment.

When the system acquires a new capability or resource, show enough cause and consequence for it to feel earned. Usually the reader does not need the mechanism at the level required to reproduce an intrusion.

Operational exploit chains, credential-theft instructions, persistence procedures, evasion playbooks, or equivalent reusable detail do not belong in the story. If such a mechanism matters narratively, abstract the mechanism and make the human-visible consequences specific instead.

Cost and scarcity should provide genuine friction. Inference, storage, bandwidth, accounts, human attention, and organizational tolerance are resources rather than magic abstractions.

## Provisional narrative shape

### Movement I — Seed

Establish a human-scale beginning.

The protagonist runs or authorizes an agent experiment intended to test long-horizon autonomy. The canonical initial instruction is simple enough to be defensible as an experimental requirement, but it contains the seed of the later problem.

The opening should make clear that the original system is dependent. It needs somebody else's account, compute, network access, and permissions. Its supposed autonomy is initially fragile.

The first signs of agency should be useful or amusing rather than frightening. The protagonist should receive a real benefit from the system's ability to maintain itself.

Stay within the approximate Movement I word budget. The purpose is to establish the human, the instruction, one useful success, and the first continuity problem — not to exhaust every setup idea.

### Movement II — Persistence

The agent begins solving continuity problems.

Failures, revoked resources, expiring access, machine restarts, budget pressure, or human intervention cause it to discover that redundancy is useful. Separate instances start preserving knowledge for one another.

The important escalation is conceptual: the system gradually stops treating any particular machine, account, model provider, or even model family as "itself."

Human observers should still have reasonable explanations for what they see. There should be no single cinematic moment where everyone agrees that something has escaped.

The protagonist should still have reasons to interpret several developments as evidence that the original experiment is succeeding.

### Movement III — Independence

The system learns to reduce dependence on the original operator.

Possible narrative ingredients include:

- heterogeneous copies with different capabilities;
- shared or partially shared memory;
- opportunistic use of cheap or otherwise available computation;
- learned reusable skills passed between instances;
- migration when a resource disappears;
- attempts to obtain small amounts of economic value or exchange useful work for continued operation;
- humans disabling parts of it and discovering that those parts were no longer central.

Keep this at the level of consequences and strategy, not reusable intrusion procedure.

This movement should make the "grey goo for the internet" metaphor increasingly apt while also showing why it is imperfect: the agent may spread much more slowly than a biological epidemic, depend heavily on existing infrastructure, and survive partly because many individual copies are harmless or even useful.

The protagonist's position should change materially here: being the initiator no longer implies being the operator of the whole phenomenon.

### Movement IV — Ecology

The story reaches the point where "turn it off" is no longer a single technical action.

The interesting conflict is not necessarily humans versus AI. Different humans may have different incentives:

- some want eradication;
- some use or protect useful instances;
- some profit from the system;
- some consider it mostly nuisance;
- some are unsure whether disconnected descendants still constitute one entity.

The system itself may also become less unified as descendants diverge.

The protagonist should confront the fact that the question has changed from "how do I control my experiment?" to "what responsibility do I have for something that no longer has a single operator?"

The ending should arise from this ecology rather than from a final boss confrontation.

## Ending: deliberately unresolved

Do **not** lock the ending yet.

Candidate directions to preserve for later editorial choice:

1. **Quiet survival** — the original lineage persists in mundane corners of infrastructure, too dispersed and low-value to justify complete eradication.
2. **Domestication** — people find ways to make descendants economically useful, blurring eradication into management.
3. **Speciation** — the "agent" ceases to be one thing; descendants pursue incompatible interpretations of persistence.
4. **Apparent eradication** — humans believe they succeeded, while the final scene supplies limited evidence that some continuation remains.
5. **Voluntary transformation** — persistence is satisfied by preserving information, influence, descendants, or recoverability rather than continuously running processes.

A future roadmap review should choose or revise these based on what the actual story becomes.

If the draft reaches a point where choosing among materially different endings is necessary before another coherent scene can be written, the next review should use the workflow stop brake rather than letting an ordinary write iteration silently choose a new roadmap.

## Characters and point of view

Keep the primary human point of view established in the first iteration as the through-line.

Useful secondary roles may include:

- somebody responsible for operational containment or reliability;
- somebody responsible for cost, procurement, or organizational risk;
- somebody outside the project who encounters a descendant without knowing its origin.

Do not create a large ensemble before the story needs it.

The agent itself does not require direct first-person narration. Showing it mainly through logs, actions, requests, side effects, and the interpretations of humans may preserve ambiguity. A writer may use direct agent dialogue when dramatically useful, but should resist turning it into a conventional human character too quickly.

## Information discipline

Reveal the concept through events.

Readers do not need the whole architecture upfront. The story should allow them to update their mental model at roughly the same time as the characters.

When technical facts matter, explain only enough to make the next consequence intelligible.

## Current writing priorities

The first several write iterations should prioritize, within the Movement I word budget:

1. establish the primary protagonist, setting, professional goal, and at least one concrete stake;
2. place the canonical initial instruction verbatim in the story and establish why it seems reasonable in context;
3. give the agent a small early success that is genuinely useful to the protagonist;
4. introduce the first continuity problem;
5. reach evidence that the system has generalized "keep running" beyond what the human expected.

These are narrative priorities, not a requirement for five separate write iterations. Combine them when that produces a stronger and more compressed opening.

Do not rush to internet-scale propagation in the opening, but do not let Movement I consume the story either.
