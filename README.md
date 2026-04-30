# agent-skills

Agent skills in https://agentskills.io/ format.

## Installation

**Using `skills` command** (available via `npx skills`):
- Docs: https://github.com/agent-skills/skills-cli

```shell
# Install globally
skills install https://github.com/antono/agent-skills --global

# Install for specific agent
skills install https://github.com/antono/agent-skills --agent claude
```

**Manual installation:**
```shell
git clone https://github.com/antono/agent-skills
cp -r agent-skills/skill-name ~/.claude/skills/
```

**Supported agents:** Claude Code (`~/.claude/skills/`), Codex (`~/.codex/skills/`), OpenCode (`~/.config/opencode/skills/`), GitHub Copilot (`~/.copilot/skills/`)
