# KraftSQL Agentic Workbench

AI agent setup to work on the [**Kraft**SQL](https://github.com/kraftsql) project.

## Structure

This repository itself contains the setup for AI agent development tools.
The other **Kraft**SQL repositories are meant to be cloned inside this workspace, so they can be searched and authored together, sharing this setup.

## Agentic tooling

The agentic tooling is kept vendor-agnostic wherever possible.
- There is a neutral [AGENTS.md](AGENTS.md).
- Skills are written using the non-proprietary [Agent Skills format](https://agentskills.io/home) and placed under [.agents/](.agents).

Individual tooling might need the to be configured to use these.

### Claude Code

Project config for [Claude Code](https://claude.com/de/product/claude-code) is already prepared in [.claude/](.claude).
To finish your local setup,
- create a symlink `.claude/skills` pointing to [.agents/skills](.agents/skills).
