# Programmatic Memory Propagation

Agents use skills as a curated subset of programmatic memory.

Raw memory can be noisy: full sessions, tool logs, failures, temporary plans, and local details. Skills are the refined layer: reusable instructions that can travel between agents, projects, and teams.

## Memory Loop

```text
session evidence
  -> reflection
  -> candidate learning
  -> existing skill search
  -> domain, base, or adapter update
  -> project install
  -> repeated use
  -> promotion
```

## Reflection Inputs

A reflection workflow may inspect:

- user goals and constraints;
- commands run and their outcomes;
- diffs and files changed;
- tests, CI, and runtime verification;
- repeated failures and recoveries;
- decisions that changed the workflow;
- user feedback after delivery.

## Extraction Rules

Prefer `skills/base` when the learning is about:

- sequencing of work;
- architectural judgment;
- review criteria;
- evidence collection;
- promotion or installation policy;
- general debugging patterns.

Prefer `skills/agents/<agent>` when the learning is about:

- tool invocation syntax;
- runtime-specific memory paths;
- connector/plugin constraints;
- required final-response directives;
- agent-specific safety rules.

Prefer `domains/<domain>` when the learning is reusable in one field, product line, or business domain, but not yet obviously cross-domain.

Prefer project-local installation when the learning contains:

- repository-specific scripts;
- private infrastructure details;
- team-only release processes;
- unstable experiments.

## Propagation Discipline

Promotion should be pull-based and evidence-based.

An agent may suggest promotion, but the organization should only promote after the workflow has survived repeated use and another maintainer can understand it without the original session context.

## Domain Branch Propagation

Domain memory should move in this order:

1. Agent private/local experiment.
2. Domain branch, such as `domain/fitness`, as the authoritative domain memory branch.
3. Global promotion branch, `ascent`.
4. Stable all-domain branch, `main`.

The `dev` branch is allowed for normal repository development, but it is not a promotion state. Promotion evidence still flows through private/local memory, domain branches, `ascent`, and `main`.

When a domain branch repeats logic that appears useful elsewhere, extract that logic into `skills/base` before promoting.

## Program Layer

A program is an installable bundle of programmatic memory. It can include:

- base skills;
- domain memory;
- agent adapters;
- scripts;
- examples;
- references;
- install metadata;
- publication metadata.

Agents should use:

- `find-program` before creating or installing;
- `install-program` to place the bundle at the right scope;
- `publish-program` to validate and propagate the bundle.
