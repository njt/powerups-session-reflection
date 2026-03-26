# powerups-session-reflection

A Claude Code skill that analyzes your recent session history to identify inefficiency patterns and propose actionable improvements.

## What it does

Runs a retrospective on your last 48 hours of Claude Code sessions. It:

1. Extracts a token-reduced summary from session files using `jq` (98% smaller than raw files)
2. Analyzes for patterns: wasted tokens, wrong paths, repeated mistakes, missing automation, missing docs
3. Proposes concrete improvements: CLAUDE.md updates, new skills/commands, scripts, git hooks, workflow changes
4. Generates a reflection document with copy-paste ready fixes

All proposals are presented for review — nothing is auto-implemented.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- `jq` installed and on PATH

## Installation

Clone this repo, then symlink the skill and command into your Claude Code config:

```bash
# Clone
git clone https://github.com/njt/powerups-session-reflection.git
cd powerups-session-reflection

# Symlink skill
ln -s "$(pwd)/skills/session-reflection" ~/.claude/skills/session-reflection

# Symlink command
ln -s "$(pwd)/commands/reflect.md" ~/.claude/commands/reflect.md
```

## Usage

In any Claude Code session, run:

```
/reflect
```

This works from any project directory — the skill auto-detects the correct session data path.

## How it works

Claude Code stores session data as JSONL files under `~/.claude/projects/`, keyed by working directory path. The skill:

1. Derives the project key from your current working directory
2. Runs a `jq` pipeline that extracts only user messages (first 300 chars), tool call names, and result sizes — keeping the summary small enough to analyze without blowing the token budget
3. Feeds the summary to a subagent that looks for known inefficiency patterns
4. Outputs a structured reflection document with prioritized, copy-paste ready improvements

## License

MIT
