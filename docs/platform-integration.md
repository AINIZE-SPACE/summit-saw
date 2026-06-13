# Platform Integration

summit-saw is also a skill/program platform.

Other agent platforms should not need to understand every internal file convention. They should be able to use three stable operations:

- `find_program`
- `install_program`
- `publish_program`

These can be implemented as MCP tools, CLI commands, or native platform adapters.

## Program Contract

A program is broader than a skill. It can contain:

- one or more skills;
- domain program memory;
- agent adapters;
- scripts;
- references;
- examples;
- install metadata;
- publish metadata.

Use `schemas/program.manifest.schema.json` as the platform-facing metadata contract.

## MCP Shape

Recommended MCP tools:

```text
find_program(query, domain?, agent?, scope?, branch?)
install_program(programRef, target, agent?, scope?, branch?)
publish_program(programRef, targetPlatform, promotionLevel?, branch?)
```

The MCP server should treat Git branches as first-class sources:

- `agent/<agent>/<domain>/<topic>`
- `domain/<domain>`
- `dev`
- `ascent`
- `main`

## Platform Adapters

Adapters should translate summit-saw programs into each platform's native shape:

- Codex skills;
- Claude Code skills or commands;
- Harness pipeline assets;
- OpenClaw plugins or skills;
- MCP registry entries.

Adapters should not fork the base workflow. If an adapter discovers reusable behavior, extract it into `skills/base`.

## One-Click Publish

One-click publish should still perform real checks:

- schema validation;
- secret scanning;
- branch target verification;
- package assembly;
- commit and push when publishing to summit-saw;
- upload or API call when publishing to external platforms;
- publication log update.

Publishing to external platforms should return a stable install reference that `install-program` can consume later.
