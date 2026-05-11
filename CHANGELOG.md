# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

- **claude-checkup**: renamed plugin from `audit-suite` to `claude-checkup`. Install command is now `/plugin install claude-checkup@buvis-plugins`; the old `audit-suite` name no longer resolves. Skill names (`audit-security`, `audit-skills`, ...) and the orchestrator command (`/audit-claude-config`) are unchanged.

## [0.1.1] - 2026-05-11

### Changed

- Helper-script paths in `audit-security`, `audit-sessions`, and `audit-skills` now use `${CLAUDE_SKILL_DIR}` (the env var Claude Code exports into the Bash tool for plugin skills) instead of hardcoded `~/.claude/skills/...` paths. Required so the scripts resolve when running from the plugin install rather than the personal skills directory.

### Added

- Bundled `validate_skill.py` inside `audit-skills/scripts/` (snapshot of the validator from the `create-skill` skill) so the audit is self-contained and does not depend on `create-skill` being installed locally.

## [0.1.0] - 2026-05-10

### Added

- Initial release with 13 audit skills:
  - `audit-claude-config` — orchestrator that runs every audit and produces a unified dashboard plus prioritized remediation plan
  - `audit-security` — hardcoded secrets, loose permissions, hook injection, risky MCP servers
  - `audit-permissions` — permission sprawl, unused grants, escalations
  - `audit-hooks` — hook health, existence, executability, silent failures, performance
  - `audit-settings` — settings conflicts across global/project/local scopes
  - `audit-mcp-health` — MCP server reachability, freshness, last-used tracking
  - `audit-plugins` — plugin freshness, stale cached versions, disk reclamation
  - `audit-memory` — memory index consistency, orphan and missing entries
  - `audit-skills` — skill structural validation, frontmatter, trigger patterns
  - `audit-rules` — rule conflicts, shadowing, redundancies, staleness
  - `audit-context` — per-component token overhead, cache classification
  - `audit-sessions` — session transcript analysis, anomalies, unused skills
  - `audit-project-orphans` — stale project configs in `~/.claude/projects/`
- Python helper scripts with full pytest coverage for `audit-security` (17 tests) and `audit-sessions` (26 tests).
