# Skill Lifecycle

This document defines how a skill moves from a private experiment to an organization-shared asset.

## States

### `personal-experiment`

Use this when a skill is based on one person's or one agent's early workflow.

Requirements:

- Clear owner.
- Clear problem statement.
- Evidence from at least one real session.
- Known limitations written down.

### `project-incubating`

Use this when the skill has helped inside one project and should be installed locally for that project.

Requirements:

- Used successfully in more than one session or by more than one agent.
- Project-specific assumptions are explicit.
- The generic part has been considered for extraction into `skills/base`.

### `team-promoted`

Use this when the skill is useful across related projects or a team workflow.

Requirements:

- At least two successful use cases across contexts.
- Failure modes and rollback instructions are documented.
- Agent-specific adapters exist if needed.

### `organization-shared`

Use this when a skill is stable enough to become a default organizational asset.

Requirements:

- Stable base skill.
- Compatibility notes for supported agents.
- Minimal install instructions.
- Review by at least one maintainer outside the original project.

## Promotion Checklist

- The skill solves a recurring problem.
- It does not encode private credentials, local-only paths, or fragile assumptions.
- It separates base logic from agent-specific execution details.
- It separates domain-specific memory from cross-domain base logic.
- It has examples or evidence from real sessions.
- It can be installed and removed without breaking the project.

## Domain Promotion Path

Domain skills and program memories move through four levels:

```text
private/local -> domain/<domain> -> ascent -> main
```

The repository may also use `dev` for ordinary development and integration work. `dev` is not a promotion level.

Use a domain branch when a skill is reusable inside one domain but not yet proven across domains.

Use `ascent` when multiple domains or maintainers agree that a skill is becoming cross-domain.

Use `main` only for stable, reviewed, cross-domain assets.

## Deprecation

Deprecate a skill when it is replaced, unsafe, misleading, or no longer used.

Do not delete immediately. Mark it deprecated in `manifest.json`, point to the replacement, and remove only after dependent projects have migrated.
