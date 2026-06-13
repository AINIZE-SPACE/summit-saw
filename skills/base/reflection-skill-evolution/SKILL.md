---
name: reflection-skill-evolution
description: Reflect on completed or ongoing agent work sessions, then create or evolve skills from reusable lessons. Use when the user asks to generate skills from sessions, run a dream/reflection mechanism, install project/global skills, or propagate useful agent memory into this skill library.
---

# Reflection Skill Evolution

This skill turns session experience into reusable skills.

Use it after meaningful work, during explicit user-requested reflection, or as part of an automatic "dream" mechanism that reviews recent sessions and proposes skill updates.

## Core Rule

Search existing skills before creating new ones.

If a lesson is generic, evolve `skills/base`. If a lesson is agent-specific, put the adapter detail under `skills/agents/<agent>`. If an adapter contains a generally useful improvement, extract that improvement back into `skills/base`.

## Inputs

Collect only the evidence needed for the reflection:

- user request and constraints;
- session transcript or summary;
- files changed;
- commands and verification results;
- failures, retries, and final outcome;
- user feedback or review comments;
- current skill library contents.

Do not copy private credentials or irrelevant raw logs into a skill.

## Workflow

1. **Frame the reflection**
   - Identify the work goal.
   - Identify what changed in behavior, knowledge, or process.
   - Separate one-off project facts from reusable operating knowledge.

2. **Search existing skills**
   - Search `skills/base` first.
   - Search the current agent adapter path, such as `skills/agents/codex`.
   - Search project-local skills if the project has its own skill directory.

3. **Classify the learning**
   - `base`: applies across agents and projects.
   - `agent-adapter`: depends on a specific agent runtime, tool format, memory path, or final-response convention.
   - `project-local`: useful only inside one repository or team workflow.
   - `defer`: interesting, but not proven enough to encode.

4. **Choose the smallest change**
   - Update an existing skill when possible.
   - Create a new skill only when no existing skill has the same purpose.
   - Avoid broad "miscellaneous" skills.

5. **Update base first when appropriate**
   - Add generic decision rules, workflows, checklists, and failure handling to `skills/base`.
   - Keep runtime-specific commands out of base.

6. **Update adapter only for agent-specific behavior**
   - Add install paths, command syntax, connector constraints, or response directives to `skills/agents/<agent>`.
   - Link back to the base skill through `manifest.json.baseSkill`.

7. **Install at the right scope**
   - Start with project-level installation for unproven skills.
   - Promote to personal/global only after repeated successful use.
   - Promote to organization-shared only after review and cross-context evidence.

8. **Record evidence**
   - Add concise evidence to `manifest.json.evidence`.
   - Prefer stable references: issue IDs, PRs, session IDs, or short summaries.

## Dream Mechanism

The dream mechanism is an automatic reflection pass. It should be conservative.

Recommended cadence:

- project-local: after significant task completion;
- personal-global: daily or weekly;
- organization-shared: human-reviewed promotion only.

Dream output should be a proposal or patch, not an unreviewed global overwrite.

## Decision Table

| Finding | Destination |
| --- | --- |
| Better sequencing for reviews, debugging, release, or planning | `skills/base` |
| Codex-only final response directive | `skills/agents/codex` |
| Claude Code command metadata | `skills/agents/claude-code` |
| Harness pipeline execution constraint | `skills/agents/harness` |
| OpenClaw plugin bridge detail | `skills/agents/openclaw` |
| Repo-specific deployment path | project-local skill |
| Single unproven anecdote | defer or personal experiment |

## Quality Bar

A skill update is good when:

- it is shorter than the raw session it came from;
- it changes future behavior;
- it names when to use and when not to use the workflow;
- it separates base logic from adapter mechanics;
- it can be understood without reading the original session.

## Installation Guidance

Install in this order unless the user asks otherwise:

1. Project-local skill directory.
2. Personal/global agent skill directory.
3. Team template or shared repository.
4. Organization registry.

Promotion is not just copying files. It requires evidence that the skill is reusable and safe.
