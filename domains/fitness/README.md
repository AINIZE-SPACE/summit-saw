# Fitness Domain

The `fitness` domain is the programmatic memory layer for fitness-related development work.

Initial source project:

```text
E:\my_project\aifitness
```

Recommended domain branch:

```text
domain/fitness
```

## Purpose

This domain captures reusable fitness-product and fitness-agent development knowledge from `aifitness`, then separates it into:

- domain-level memory that belongs in `domains/fitness`;
- cross-domain workflow that should be extracted to `skills/base`;
- agent-specific behavior that belongs in `skills/agents/<agent>`;
- project-only details that should remain local to `aifitness`.

## Current Seed Areas

- Sprint planning around fitness training-loop and polish/deploy specs.
- Fitness project automation with scheduled status and monitoring workflows.
- GitHub issue, PR, regression, and release-readiness routines from `aifitness`.

## Boundaries

Do not promote these directly to global assets:

- OAuth credentials or local credential paths.
- Current board, issue, or PR state without re-verification.
- One-off project paths that only work in `E:\my_project\aifitness`.
- Discussion-only planning outcomes that the user has not approved for execution.

## Promotion Path

```text
agent/<agent>/fitness/<topic>
  -> domain/fitness
  -> ascent
  -> main
```

When a fitness-domain rule becomes useful outside fitness, extract it into `skills/base` before promoting.
