# Tracker Boot Adapter

Read this only when applying the general story-authoring workflow to Tracker Boot. Product behavior
can change; if a live tool response differs, prefer the current behavior and report the difference.

## Types and formats

- Supported types: Feature, Design, Bug, Chore, Release.
- Choose a type from the nature of the work, not its wording.
- Apply the core templates for the selected type. Do not shorten a Feature or Design story merely
  because the tool schema mentions only acceptance criteria and notes.
- Keep optional evidence and unknowns under Notes; they do not replace the required type-specific
  sections.

## Read and write sequence

1. List projects first when the project ID is unknown.
2. Search existing stories by title and key terms before creation.
3. Before editing, read the story, tasks, and comments to separate requirements from progress.
4. Read project members before assigning owner IDs.
5. Change only the requested field and scope.
6. Record progress and results as comments instead of replacing requirements.
7. Read the story back and verify type, description, relationships, owners, and state.

## Constraints to check

- Chores do not offer Finished or Delivered. Read the states available in the live project and use
  its valid completion path after Started.
- A `#number` in a description may create a story reference or relationship. Read back the story
  and check for unintended relationships after adding a numeric ID.
- An early-stage Feature may keep the story shape, but do not invent a product concept or acceptance
  criteria. Follow the core skill's `TBD` rule and name the decision required.
- If a tool returns an access-scope error, do not route around it with another write tool. Resolve
  project access first.
