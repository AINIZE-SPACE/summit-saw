---
name: publish-program
description: Publish a program to summit-saw or another skill platform. Use when the user asks to release, promote, push, submit, or one-click publish a skill/program.
---

# Publish Program

Use this skill to publish a program after it has been found, installed, or evolved.

## Publication Targets

- summit-saw project branch.
- Domain branch, such as `domain/fitness`.
- `ascent`.
- `main`.
- External agent skill platforms.
- MCP-based registries.

## Workflow

1. Identify the program and target platform.
2. Validate manifests and required files.
3. Confirm promotion level:
   - agent private/local;
   - domain;
   - ascent;
   - main;
   - external platform.
4. Run available checks.
5. Commit changes with a focused message.
6. Push the target branch.
7. Create a PR when moving across promotion levels.
8. Publish or upload to external platforms when supported.
9. Record publication evidence.

## One-Click Publish Contract

A one-click publish tool should still perform these steps internally:

- discover target platform capabilities;
- validate schema;
- verify no secrets are included;
- package required files;
- push or upload;
- return a stable install reference;
- write a publication log.

## Platform Adapters

Keep platform-specific details outside this base skill. Examples:

- Codex skill install path;
- Claude Code skill metadata;
- Harness pipeline artifact rules;
- OpenClaw plugin bridge;
- MCP registry schema.

When a platform workflow repeats across adapters, extract it back into this base skill.
