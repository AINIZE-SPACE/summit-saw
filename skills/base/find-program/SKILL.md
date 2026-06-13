---
name: find-program
description: Find an existing program before creating or installing one. Use when an agent needs to discover skills, domain memory, adapters, scripts, or publishable bundles across branches and skill platforms.
---

# Find Program

Use this skill before creating, installing, or publishing a program.

A program is an installable bundle of programmatic memory. It may include skills, domain memory, agent adapters, scripts, references, examples, and platform metadata.

## Search Order

1. Search the current project.
2. Search the active branch.
3. Search the relevant domain branch, such as `domain/fitness`.
4. Search `ascent`.
5. Search `main`.
6. Search configured external skill platforms.

Do not create a new program until existing candidates have been checked.

## Inputs

- Domain name, if known.
- Agent platform, if relevant.
- Desired install scope.
- User goal.
- Keywords from the current task.

## Workflow

1. Identify the target domain and agent platform.
2. List local program candidates from `domains/`, `skills/base`, and `skills/agents`.
3. Fetch or inspect relevant Git branches when available.
4. Prefer domain-specific programs for domain work.
5. Prefer base programs only when the workflow is clearly cross-domain.
6. Report the best match, install target, source branch, and confidence.

## Match Quality

Classify results as:

- `exact`: same domain, same purpose, compatible agent/platform.
- `adaptable`: same workflow, different domain or platform.
- `base-candidate`: generic enough to extract or reuse from base.
- `none`: no safe match found.

## Do Not Use When

- The user explicitly gives a specific program path to install.
- The task is a one-off local command with no reusable program memory value.
