# cc-rr-changes

This repo is updated by a scheduled Claude Code trigger that reviews
Claude Code releases for red-run impact.

## Trigger behavior

On each run:
1. Read the most recent report in `reports/` (sort by filename, newest first).
   Extract the version numbers from the "New Releases Reviewed" section —
   these are the already-reviewed versions. If no reports exist, treat all
   releases as new.
2. Fetch releases via `gh api repos/anthropics/claude-code/releases --paginate -q '.[:10]'`
   (use `gh api` not WebFetch — it's authenticated and returns structured JSON).
3. Filter out any release whose `tag_name` appears in any existing report's
   "New Releases Reviewed" list. Only analyze genuinely new releases.
4. If no new releases: print "No new Claude Code releases since vX.Y.Z" and stop.
   Do NOT create an empty report. Do NOT commit.
5. For new releases: summarize changes and analyze red-run impact.
6. Write report to `reports/YYYY-MM-DD.md`. If a report for today already
   exists (multiple runs same day), append a suffix: `YYYY-MM-DD-2.md`.
7. Commit and push.

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
