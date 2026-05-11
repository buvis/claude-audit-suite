# Claude Audit Suite

[![GitHub license](https://img.shields.io/github/license/buvis/claude-audit-suite)](https://github.com/buvis/claude-audit-suite/blob/master/LICENSE)

A health-check toolkit for [Claude Code](https://claude.ai/code). Thirteen focused audit skills plus a single orchestrator that runs them all, prints a dashboard, and produces a prioritized remediation plan.

## What it audits

| Skill | Catches |
|-------|---------|
| `audit-security` | Hardcoded secrets, loose permission patterns, hook injection, risky MCP servers |
| `audit-permissions` | Permission sprawl, unused grants, escalations |
| `audit-hooks` | Missing scripts, non-executable hooks, silent failures, slow hooks |
| `audit-settings` | Conflicts across global / project / local scopes, redundant overrides |
| `audit-mcp-health` | Disconnected MCP servers, stale config, last-used tracking |
| `audit-plugins` | Stale cached plugin versions, unused installs, disk reclaimable |
| `audit-memory` | Orphan memories, missing entries in `MEMORY.md` index |
| `audit-skills` | Skill structural validation, frontmatter, trigger patterns |
| `audit-rules` | Conflicts, shadowing, redundancies, staleness across rule files |
| `audit-context` | Per-component token overhead, cache classification |
| `audit-sessions` | Patterns, anomalies, unused skills across past sessions |
| `audit-project-orphans` | Stale project configs in `~/.claude/projects/` |
| `audit-claude-config` | **Orchestrator** — runs them all, prints dashboard, builds remediation plan |

## Install

Two commands inside Claude Code:

```
/plugin marketplace add buvis/claude-plugins
/plugin install audit-suite@buvis-plugins
```

Restart Claude Code, then run `/audit-claude-config` to get a full health report.

### Update

```
/plugin update audit-suite@buvis-plugins
```

### Alternative: install directly from this repo

```
/plugin marketplace add buvis/claude-audit-suite
/plugin install audit-suite@claude-audit-suite
```

## Usage

```
audit my claude config        # run everything, build remediation plan
audit security                # security-only audits
audit health                  # health-only audits
audit efficiency              # efficiency audits (slower)
```

Or invoke any individual audit by name, e.g. `audit hooks`, `audit memory`, `audit skills`.

The orchestrator saves a dated report to `dev/local/audit-results/{YYYY-MM-DD}.md` and diffs against the previous report (new findings, resolved findings, unchanged).

## Severity grading

Each finding gets a severity, prioritized in the remediation plan:

- **CRITICAL** — fix now (data loss, secret exposure, broken hooks)
- **HIGH** — fix this week (permission escalations, conflicting rules)
- **MEDIUM** — fix when convenient (cleanup opportunities)
- **LOW** — backlog (style, minor consistency)

## Requirements

- Claude Code with plugin support
- `python3` on PATH (helper scripts in `audit-security` and `audit-sessions`)
- Optional: [warden](https://github.com/buvis/claude-warden) — if installed, the orchestrator also calls `/warden:review-decisions`

## License

MIT
