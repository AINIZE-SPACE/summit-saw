# Domain Branching

This repository is branch-first for domains.

Git branches are the primary boundary for domain memory. `domains/<domain>` directories are the asset layout inside a branch, not the main isolation mechanism.

The main reason is cleanliness: a domain can evolve, rewrite, experiment, and accumulate project-specific memory on its own branch without forcing every other domain to carry that noise.

## Branch Levels

| Level | Branch pattern | Purpose |
| --- | --- | --- |
| Agent private/local | `agent/<agent>/<domain>/<topic>` | Local or private experiments by one agent. Push only when intentionally shared. |
| Domain shared | `domain/<domain>` | The common memory branch for one domain. |
| Development integration | `dev` | Normal repository development and integration branch. It does not mean global promotion is complete. |
| Ascent | `ascent` | Cross-domain promotion branch for memory climbing from domain-specific to global. |
| Global stable | `main` | Reviewed all-domain shared assets. |

## Why Branch-First

Use one branch per domain because:

- Git history becomes the domain's evolution log.
- Domain-specific experiments do not pollute `main`.
- Promotion is explicit through merges or PRs.
- Conflicting domain vocabulary can coexist until it is reconciled.
- Agents can install or search one domain without scanning unrelated domains.
- A domain can be archived, forked, or reset using native Git mechanics.

The tradeoff is operational: agents need reliable branch fetch/search/install behavior. That is why `find-program`, `install-program`, and `publish-program` exist as platform-facing skills.

## Domain Asset Layout

Each domain branch should expose this layout:

```text
domains/<domain>/
+-- README.md
+-- manifest.json
+-- program-memory.md
+-- promotion-log.md
+-- skills/
```

The directory can exist on multiple branches, but the branch decides its authority:

- On `agent/<agent>/<domain>/<topic>`, it may contain unstable private notes.
- On `domain/<domain>`, it is the authoritative domain memory.
- On `dev`, it may contain repository development work before promotion classification.
- On `ascent`, it contains only cross-domain candidates extracted from domain branches.
- On `main`, it contains only stable, reviewed global assets.

`main` should not become a warehouse of every domain's private memory. Domain-private or domain-only memory should live on its domain branch.

## Promotion Rules

Promote one level at a time:

```text
private/local -> domain/<domain> -> ascent -> main
```

Use `dev` for ordinary repository integration work. Do not treat a merge into `dev` as evidence that a skill is ready for global promotion.

Before promotion:

- Search existing base, domain, and agent skills.
- Extract cross-domain logic into `skills/base`.
- Keep domain vocabulary and domain-specific playbooks under `domains/<domain>`.
- Keep runtime mechanics under `skills/agents/<agent>`.
- Record evidence in `manifest.json` and `promotion-log.md`.

## Find, Install, Publish

Agents should interact with programs through three base workflows:

- `find-program`: discover programs across branches, domains, skills, and platform registries.
- `install-program`: install a program into a project, agent runtime, domain branch, or global scope.
- `publish-program`: validate, commit, tag, push, and publish a program to this repository or external skill platforms.

A program is broader than a skill. It may include skills, domain memory, agent adapters, scripts, references, and install metadata.

## Fitness Domain

The first domain is `fitness`.

Recommended branch:

```text
domain/fitness
```

Seed source:

```text
E:\my_project\aifitness
```

The `fitness` domain starts as a programmatic memory projection from the `aifitness` project. It should preserve useful fitness-domain workflows while keeping project-only paths, credentials, and current issue states out of global assets.
