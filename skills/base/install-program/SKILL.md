---
name: install-program
description: Install a program into the correct project, domain, agent, or global scope. Use after find-program selects a candidate or when the user asks to install a skill/program for an agent platform.
---

# Install Program

Use this skill to install an existing program safely.

## Core Rule

Install at the narrowest useful scope first.

Default path:

```text
project -> private/local -> domain branch -> ascent -> main
```

Do not install globally just because a program exists.

## Inputs

- Program source.
- Target project or repository.
- Target agent platform.
- Target scope.
- Domain branch, if relevant.
- Required files and scripts.

## Workflow

1. Confirm the program source and target scope.
2. Read the program manifest.
3. Check agent compatibility.
4. Check whether installation requires a branch checkout, copy, symlink, package install, MCP registration, or platform upload.
5. Install into the narrowest useful target.
6. Verify that the target agent can discover the program.
7. Record install evidence in the target project or domain log.

## Install Targets

- `project`: local to one repository.
- `private/local`: private to one agent runtime, developer machine, or project.
- `domain`: installed or referenced from `domain/<domain>`.
- `ascent`: staged for cross-domain adoption.
- `main`: stable, reviewed, all-domain program.
- `external-platform`: published to another skill platform.

## Safety Checks

- Do not copy secrets.
- Do not overwrite user-local agent configuration without confirmation.
- Do not assume branch state is current; fetch or verify when possible.
- Keep platform-specific details in adapters.

## Output

Report:

- installed program;
- source branch/path;
- target scope;
- verification performed;
- any remaining manual step.
