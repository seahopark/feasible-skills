---
name: story-authoring
description: |
  Writes or reviews executable product stories from ideas, feature requests, design work, bugs,
  chores, and releases. Use before creating or revising an issue when the work needs current-state
  verification, type selection, story splitting, user-value framing, acceptance criteria, and a
  duplicate check. Use user-story-mapping to organize many stories around a journey. When available,
  use story-autopilot to decide ownership and execution after a story is accepted.
---

# Story Authoring

Turn work into a **decision-ready, verifiable unit**, not merely a completed template. Accurate
current state, user value, and explicit unknowns matter more than plausible prose.

## 1. Confirm the request and target tracker

Determine whether the user wants a draft, a review, a new story, or an edit to an existing story.
If the tracker and project are already named, use them as the write target. Otherwise, draft
first and do not write to an arbitrary external system.

Done when: the output form is known, and the target tracker and project are known if a write was
requested.

## 2. Establish current truth

When the conversation is not sufficient, inspect only the evidence needed for the story:

- the existing story's description, tasks, comments, relationships, and state;
- product specs, decision records, and recent work notes;
- actual code, deployment state, and recent commits;
- reproduction steps, logs, or screens for a bug.

Do not infer the current state from one old idea document. Do not describe an implemented or
deployed feature as a new idea. Resolve conflicting evidence against the latest reliable source.
Mark anything unverifiable as `TBD` or `needs confirmation`; never invent a fact to complete a
template.

Done when: you can state the current-state premise and its evidence.

## 3. Choose the work type

Classify by the nature of the result, not by how the request was phrased.

- **Feature:** a new behavior or value available to a user or customer
- **Design:** a result that defines or validates a user experience or interaction
- **Bug:** a reproducible gap between expected and actual behavior
- **Chore:** internal or operational work without direct user value
- **Release:** a delivery unit that brings a set of changes to users or a market

When two types seem plausible, use the final artifact and completion test to choose. If the type
changes the workflow or available states and remains ambiguous, ask instead of guessing.

Done when: the type and its rationale fit in one sentence.

## 4. Split to one verifiable outcome

One story should describe one independently verifiable outcome. Split when:

- different users or goals appear;
- one part could deliver value without the other;
- discovery, decision, implementation, and release are mixed together;
- acceptance criteria become unrelated scenario groups; or
- one unknown prevents every other part from being completed.

Do not create valueless technical Features merely to make each implementation step small. Keep
non-independent steps as tasks inside the story. For a large undecided item, separate the decision
boundary from later execution before inventing implementation stories.

Done when: the outcome can be stated in one sentence without joining separate results.

## 5. Write for the selected type

Use the exact formats in [references/templates.md](references/templates.md).

For Features and Design stories, keep these roles distinct:

- **Why:** the problem, evidence, and reason to act now
- **As a:** the specific person or role using the result
- **I want:** that person's goal
- **So that:** the value gained from reaching the goal
- **Acceptance Criteria:** externally observable completion conditions
- **Notes:** constraints, evidence, relationships, and unknowns

Do not repeat `Why` as `So that`. Write a user goal in `I want`, not a screen or implementation
mechanism. Describe observable behavior in acceptance criteria, not internal implementation.

For a Bug, separate reproduction steps, expected behavior, and actual behavior. If it has not been
reproduced, say so and record the missing evidence in Notes. Use plain Markdown for Chores and
Releases; do not add Gherkin unless explicitly requested.

Done when: every required section for the chosen type exists and serves a distinct purpose.

## 6. Validate acceptance criteria

Write Feature and Design acceptance criteria as Scenario / Given / When / Then.

- One Scenario covers one user situation.
- Given contains only required starting conditions.
- When is one central action or event.
- Then is an observable result such as a screen, state, or data change.
- Include the main flow and only boundaries that materially affect completion.
- Do not require function names, components, or libraries.
- Do not specify a condition the team cannot actually verify.

When the concept or policy is undecided, do not fabricate behavior. Mark it as `TBD — define the
concept and success criteria before implementation`. Do not disguise the unknown itself as a
finished feature's acceptance criterion.

Done when: each Scenario is independently testable, or the blocking unknown is explicit.

## 7. Run the quality gate

Check all of the following:

- The title communicates the outcome.
- The story does not rely on stale current-state assumptions.
- The user, goal, and value form a logical chain.
- `Why`, `I want`, and `So that` do different jobs.
- Acceptance criteria are observable and testable.
- Unknowns and hypotheses are not presented as facts.
- The story does not duplicate existing work.
- It does not combine multiple independent outcomes.
- Progress notes or retrospectives are not mixed into requirements.
- Notes do not contradict acceptance criteria.

Do not hide a failed check with smoother wording. Inspect more evidence, split the story, or leave
an explicit question.

Done when: every check passes or every unresolved check is visible.

## 8. Persist and read back

If the user asked only for a draft or review, return it without writing to an external system. If
creation was requested, search for duplicates by title and key terms, then persist it. When editing,
change only the requested scope. Record progress as a comment rather than replacing requirements.

After writing, read back the title, type, description, relationships, owners, and state. Apply a
tracker-specific adapter when one exists. For Tracker Boot, also read
[references/tracker-boot.md](references/tracker-boot.md).

Done when: the stored story matches the reviewed draft and has no unintended relationships or
field changes.

## Output

Unless the user requests another format, present:

1. selected type and rationale;
2. split decision and rationale;
3. paste-ready story body;
4. facts or decisions that still need confirmation.
