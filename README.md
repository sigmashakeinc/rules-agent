# rules-agent

AI agent efficiency and cost-reduction rules. Redirects shell commands to native agent tools (Read, Grep, Glob, Edit, Write), and prevents context-bloat patterns that waste LLM tokens — reducing file-operation costs by 30–50%.

**13 rules · 3 files**

![rules-agent — AI agent efficiency and tool substitution demo](demo.cast)

> [▶ Watch interactive demo on SigmaShake Hub](https://hub.sigmashake.com/ruleset/rules-agent)


## Install

```bash
ssg hub pull rules-agent
```

Available on the [SigmaShake Hub](https://hub.sigmashake.com) — the open registry for AI agent governance rules. Compatible with Claude Code, GitHub Copilot, Cursor, Windsurf, Aider, and any AI coding agent using the `ssg` hook protocol.

## Rules

### agent_exec_tool_substitution.rules — Command-to-tool redirection (5 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-cat-use-read` | FORCE | warning | `cat file` → use the Read tool |
| `no-find-use-glob` | FORCE | warning | `find . -name` → use the Glob tool |
| `no-grep-use-grep-tool` | FORCE | warning | `grep`/`rg` → use the Grep tool |
| `no-sed-inplace-use-edit` | FORCE | warning | `sed -i` → use the Edit tool |
| `no-echo-to-file-use-write` | FORCE | warning | `echo > file` → use the Write tool |

### agent_any_efficiency.rules — Cost and hygiene controls (5 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-head-tail-use-read` | FORCE | warning | `head`/`tail` → use Read with offset/limit |
| `ask-package-install` | ASK | warning | Confirms package name before installing |
| `ask-write-temp-root` | ASK | warning | Flags temp files written to repo root |
| `log-background-agent-process` | LOG | info | Logs background processes for cleanup tracking |
| `log-agent-tool-invocation` | LOG | info | Logs sub-agent spawns for token cost awareness |

### agent_write_context_bloat.rules — Token waste prevention (3 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `ask-hook-echo-on-allow` | ASK | warning | Warns when hooks echo on every allowed tool call (~1,000 tok/session waste) |
| `ask-claudemd-duplicate-table` | ASK | warning | Flags skills/commands tables written to CLAUDE.md that may be duplicated elsewhere |
| `log-skill-file-write` | LOG | info | Audits SKILL.md writes and reminds to keep skills under 300 lines |

## Why this matters

Each shell subprocess an AI agent spawns adds latency and consumes LLM tokens in the output stream. Using native tools (Read, Grep, Glob, Edit, Write) instead of their shell equivalents:

- **Reduces token usage by ~30–50%** for file operations — native tools return structured output, not raw terminal noise
- **Provides structured output** (line numbers, file paths) without parsing — agents get clean diffs in the UI instead of raw terminal output
- **Eliminates shell escaping edge cases** that cause agents to retry operations
- **Prevents context bloat** from hooks echoing on every allowed call — a hidden cost that adds ~1,000 tokens per session

## Compatible with

- Claude Code (native Read/Grep/Glob/Edit/Write tools)
- Any AI coding agent with file operation tools
- Works with any project structure and language

## About

Part of the [SigmaShake Hub](https://hub.sigmashake.com) — open-source governance rules for AI coding agents.
Install the `ssg` CLI to enforce these rules: `npm install -g @sigmashake/ssg`
