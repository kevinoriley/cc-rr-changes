# Claude Code / red-run Change Tracker

Automated daily analysis of [Claude Code](https://github.com/anthropics/claude-code) releases for impact on [red-run](https://github.com/blacklanternsecurity/red-run).

Reports are committed to `reports/YYYY-MM-DD.md` by a scheduled Claude Code trigger.

## What it tracks

- All Claude Code release changes (features, fixes, breaking changes)
- **High impact**: agent teams API, MCP protocol, hooks, skills, permissions, task system
- **Medium impact**: tool execution, new capabilities, performance, model changes
- **Low impact**: features red-run could leverage, bug fixes, documentation

## How it works

A Claude Code [scheduled trigger](https://docs.anthropic.com/en/docs/claude-code) runs daily, fetches the latest releases from the GitHub API, compares against the last report in this repo, and commits a new analysis.
