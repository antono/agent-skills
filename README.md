# agent-skills

Agent skills in https://agentskills.io/ format.

## Installation

**Using `skills` command** (available via `npx skills`):
- Docs: https://github.com/agent-skills/skills-cli

```shell
skills install https://github.com/antono/agent-skills --global
```

**Manual installation:**
```shell
git clone https://github.com/antono/agent-skills
cp -r agent-skills/skill-name ~/.agents/skills/
```

## Skills

| Skill | Description |
|-------|-------------|
| [ck-search](ck-search/) | Powerful multi-mode code search with 'ck': fast lexical (grep/ripgrep-style) patterns, embeddings-based semantic retrieval to find code by meaning, and hybrid (semantic+keyword) fusion |
| [search-nix-manix](search-nix-manix/) | Use when working on .nix files, flakes, Home Manager configs, or NixOS configurations and need to look up options, packages, or lib functions |
