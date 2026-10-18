# KraftSQL Agentic Workbench

AI agent setup to work on the [**Kraft**SQL](https://github.com/kraftsql) project.

## Structure

This repository itself contains the tool setup to work on **Kraft**SQL, especially to work with AI agents.

- [.agents/](.agents): general, vendor-agnostic AI tooling configuration
- [.claude/](.claude/), etc.: extra proprietary config for certain AI tools
- [.adr/](.adr): Architecture Decision Records (ADRs)

The other **Kraft**SQL repositories are meant to be cloned inside this workspace, so they can be searched and authored together, sharing this setup.

## Development workflow

The suggested workflow for a feature or a change of behaviour consists of four steps.
Each step can be performed by a human or by the corresponding [skill](.agents/skills), if available.

1. **Specify** ([specify](.agents/skills/specify/SKILL.md)):
   write the spec into the code itself, by user-facing API, KDoc, documentation and unimplemented test cases.
   Commit it as `Specify: <feature>`.
2. **Plan** ([plan](.agents/skills/plan/SKILL.md)):
   starting from the spec commit, mark the necessary code changes with `// TODO(plan) step N: ...` comments,
   split into self-contained steps, beginning with the reference test.
3. **Implement**: work through the planned steps one by one, until all spec'ed test cases pass and no `TODO(plan)` is left.
4. **Review**: review the implementation against the spec.

## Agentic tooling

The agentic tooling is kept vendor-agnostic wherever possible.
- There is a neutral [AGENTS.md](AGENTS.md).
- Skills are written using the non-proprietary [Agent Skills format](https://agentskills.io/home) and placed under [.agents/](.agents).

Individual tooling might need the to be configured to use these.

### Claude Code

Project config for [Claude Code](https://claude.com/de/product/claude-code) is already prepared in [.claude/](.claude).
To finish your local setup,
- create a symlink `.claude/skills` pointing to [.agents/skills](.agents/skills).
