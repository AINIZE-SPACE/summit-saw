# Install Targets

Skills should be installed gradually. A skill becomes more widely installed only when evidence supports it.

## Project Local

Use project-local installation first when:

- the skill is new;
- the workflow depends on one repository;
- the team is still testing the process;
- the adapter details are not stable.

Project-local skills are safest for experimentation because they do not change global agent behavior.

## Personal Global

Use personal-global installation when:

- one developer or one agent repeatedly uses the skill;
- the skill is useful across that person's projects;
- the risk of global behavior change is understood.

Personal-global installation is still not organization approval.

## Team

Use team installation when:

- multiple team members use the same workflow;
- project-local copies have converged;
- the skill has examples and known failure modes.

## Organization

Use organization installation when:

- the base skill is stable;
- supported agent adapters are documented;
- promotion has been reviewed;
- rollback or deprecation paths are clear.

## Promotion Rule

Move one level at a time:

```text
project -> personal-global -> team -> organization
```

Skipping levels is allowed only when the user explicitly asks and the risk is low.
