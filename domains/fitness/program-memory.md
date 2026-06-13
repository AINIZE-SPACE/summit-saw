# Fitness Program Memory

This file is the first projection of `aifitness` program memory into the `fitness` domain.

Facts here may be memory-derived and should be re-verified before execution if they depend on current repo state, external services, credentials, dates, issue state, or branch state.

## Source Project

```text
E:\my_project\aifitness
```

## Reusable Domain Knowledge

### Sprint Planning

- When planning Sprint 3 in `aifitness`, start from:
  - `specs/012-sprint3-polish-deploy.md`
  - `specs/012-sprint3-training-loop.md`
- The two specs previously had different feature emphases and reused SK-09..SK-13 for different features, so combined plans need numbering and scope reconciliation before execution.
- If the user marks a thread as discussion-only, keep it at option/comparison level until they explicitly approve execution.

### Automation

- When native automation tooling is unavailable, a reliable Windows fallback is a PowerShell script plus Windows Scheduled Task.
- For scheduled workflows on this machine, prefer `powershell.exe` over `pwsh` unless `pwsh` is explicitly confirmed available.
- For project monitoring, hourly polling is an acceptable default when event-driven triggers are unclear.
- Initialize JSON state containers defensively before indexing nested properties.

### Repository Maintenance

- For concrete bugfix requests, default to implementation plus verification rather than analysis only.
- Local `.issues` files can record regression-ready notes, but they do not replace remote GitHub issue updates when the user asks for GitHub issue state changes.
- Use `gh` to verify remote issue and PR state when the task depends on GitHub truth.
- For large PR review, inspect key source files directly when the monolithic diff is too noisy.

## Candidate Extractions To Base

These look cross-domain and should be considered for `skills/base`:

- Discussion-only boundary handling.
- Scheduled automation fallback: local script plus OS scheduler.
- Remote issue state verification before claiming issue closure.
- One-level-at-a-time promotion of program memory.

## Keep Domain-Local

These should remain in `domains/fitness` unless repeated elsewhere:

- Fitness training-loop planning conventions.
- `aifitness` spec reconciliation rules.
- Fitness-specific project monitor semantics.

## Do Not Promote Without Re-Verification

- Current Trello board state.
- Current GitHub issue or PR state.
- OAuth credential status.
- Exact branch names in `aifitness`.
- Current deployment or CI status.
