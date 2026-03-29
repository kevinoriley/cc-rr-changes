# cc-rr-changes

This repo is updated by a scheduled Claude Code trigger that reviews
Claude Code releases for red-run impact.

## Trigger behavior

On each run:
1. Fetch `https://api.github.com/repos/anthropics/claude-code/releases?per_page=10`
2. Read the latest report in `reports/` to determine last-reviewed version
3. For new releases: summarize changes and analyze red-run impact
4. Write report to `reports/YYYY-MM-DD.md`
5. Commit and push

## Report format

Each report categorizes changes by red-run impact:
- **High**: agent teams, MCP, hooks, skills, permissions, tasks — may break red-run
- **Medium**: tool behavior, new capabilities, performance — may affect red-run
- **Low**: features to leverage, bug fixes — informational

## What counts as red-run impacting

High impact (requires action):
- Agent teams API changes (TeamCreate, SendMessage, teammate lifecycle)
- MCP protocol changes (tool registration, SSE transport, server lifecycle)
- Hook system changes (SubagentStop, TeammateIdle, SessionStart)
- Skill loading/invocation changes
- Permission model changes (allow/deny, sandbox)
- Task system changes (TaskCreate, TaskUpdate, TaskList)
- Agent tool changes (subagent_type, team_name, name params)
- Model selection/override behavior
- Context window handling (compression, memory)

Medium impact (monitor):
- Tool execution changes (Bash, Read, Write, Edit)
- New tools or capabilities
- Performance/token/rate limit changes
- Default model changes
- UI changes affecting AskUserQuestion

Low impact (informational):
- New features red-run could leverage
- Bug fixes resolving known workarounds
- Documentation or API changes
