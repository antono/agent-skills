---
name: search-nix-manix
description: Use when working on .nix files, flakes, Home Manager configs, or NixOS configurations and need to look up options, packages, or lib functions
---

# Search Nix Options with Manix

## Overview

Manix is a fast CLI for searching NixOS options, Home Manager options, nixpkgs
docs, and lib functions. Replaces web search for Nix documentation.

## When to Use

- Editing `.nix` files and need option descriptions or types
- Working on flakes and need to understand `mkDerivation`, `lib` functions
- Configuring Home Manager (`home.nix`) and need option details
- Configuring NixOS (`configuration.nix`) and need type/default/example
- Searching for nixpkgs packages or `lib` function signatures

**When NOT to use:** For package installation (use `nix search`), for building
(use `nix build`)

## Quick Reference

| Task | Command |
|------|---------|
| Search all sources | `manix <query>` |
| Exact match | `manix --strict <option.path>` |
| NixOS options only | `manix <term> --source nixos_options` |
| nixpkgs packages | `manix <term> --source nixpkgs_tree` |
| nixpkgs lib functions | `manix <term>` (searches nixpkgs by default) |
| Refresh cache | `manix --update-cache <query>` |

## Usage

### Basic Search
```bash
manix "firewall"           # substring match across all sources
manix networking.firewall   # finds NixOS options
manix mapAttrs             # finds lib.mapAttrs in nixpkgs
```

Output shows: option path, description, type, default.

### Source Filtering (QUERY comes FIRST)
```bash
# NixOS configuration work
manix services.openssh --source nixos_options

# nixpkgs package search
manix "git" --source nixpkgs_tree

# Search multiple sources
manix "networking" --source nixos_options --source nixpkgs_tree
```

**Valid sources:** `nixos_options`, `nixpkgs_tree`, `nixpkgs_comments`, `nd_options`

**Note:** `hm_options` requires Home Manager channel configured. `nixpkgs_doc` cache often empty - use default search instead.

### Strict Mode
After finding the exact attribute path:
```bash
manix --strict networking.firewall.enable
```
Prevents false positives in automation.

### Cache Management
```bash
manix --update-cache "query"    # refresh cache for query type
```
Cache location: `~/.cache/manix/`

**Note:** `--update-cache` requires a query argument (not standalone).

## Common Mistakes

| Mistake                          | Fix                                            |
| -------------------------------- | ---------------------------------------------- |
| Searching `lib.mkDerivation`     | Use `manix mkDerivation` (it's in nixpkgs_doc) |
| Using web search for Nix options | Use `manix` directly                           |
| Not filtering sources            | Add `--source` to narrow results               |
| Expecting JSON output            | manix outputs text, parse by `───` separators  |

## Limitations

### nixvim Options
Manix does NOT natively support searching nixvim options.

**Workaround:** Enable man page generation in nixvim config:
```nix
programs.nixvim = {
  enable = true;
  enableMan = true;  # Generates man pages for nixvim options
};
```
After rebuilding, try: `manix <nixvim-option>` (may work with generated man pages).

**Better alternative:** Use nixvim's [Option Search](https://nix-community.github.io/nixvim/) in the docs sidebar.

## Installation

Available in nixpkgs (unstable):

```bash
nix-env -f '<nixpkgs>' -iA manix
# or
nix run github:mlvzk/manix
```

Add to NixOS: `environment.systemPackages = [ pkgs.manix ];` Add to Home
Manager: `home.packages = [ pkgs.manix ];`
