---
name: skill-library-governance
description: Apply governance rules for this skill library, including naming, scope, promotion, deprecation, and whether knowledge belongs in base, adapter, or project-local skills.
---

# Skill Library Governance

Use this skill when creating, reviewing, promoting, or deprecating skills in this repository.

## Naming

Use concrete kebab-case names:

- `reflection-skill-evolution`
- `release-readiness-review`
- `github-ci-failure-triage`

Avoid vague names:

- `misc`
- `common`
- `helpers`
- `agent-stuff`

## Required Files

Each skill must include:

- `SKILL.md`
- `manifest.json`

Optional files:

- `examples/`
- `references/`
- `scripts/`

## Base Versus Adapter

Put content in `skills/base` when it describes reusable reasoning, workflow, checks, or decision rules.

Put content in `skills/agents/<agent>` when it depends on:

- runtime-specific tools;
- memory paths;
- command syntax;
- plugin or connector mechanics;
- final-response directives;
- installation layout.

If an adapter becomes useful outside one agent, extract the generic part into base.

## Scope Governance

Use these scopes:

- `personal-experiment`
- `project-incubating`
- `team-promoted`
- `organization-shared`

New skills should usually start as `personal-experiment` or `project-incubating`.

## Review Checklist

Before merging a skill update:

- The skill has a single clear purpose.
- It does not duplicate an existing skill.
- The scope matches the evidence.
- Agent-specific details are not hidden in base.
- The manifest is valid.
- The instructions are actionable and concise.
- The skill says when to use it.

## Deprecation

When replacing a skill:

1. Mark `manifest.json.status` as `deprecated`.
2. Set `deprecatedBy`.
3. Add a short note at the top of `SKILL.md`.
4. Keep the deprecated skill until downstream users have migrated.
