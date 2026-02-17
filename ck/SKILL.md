---
name: ck-grep-search
description: "Powerful multi-mode code search with 'ck': fast lexical (grep/ripgrep-style) patterns, embeddings-based semantic retrieval to find code by meaning, and hybrid (semantic+keyword) fusion. Includes usage examples, indexing notes, scoring details, and threshold guidance."
---

# ck search

This skill explains how to use the `ck` tool to do semantic, hybrid, or classic
lexical searches across a project repository. `ck` combines three complementary
modes:

- Lexical: fast pattern/regex matching (like `grep` / `rg`).
- Semantic: embeddings-based retrieval to find code "by meaning" even when
  terminology differs.
- Hybrid: combines semantic relevance with keyword constraints (reciprocal
  rank fusion) to get precise results.

`ck` is a drop-in replacement for `grep`/`rg` for everyday pattern searches,
and it adds powerful semantic and hybrid modes once you index the repo.

Very short: Use when grep misses related code or you want to find code by
meaning (semantic) or combine keywords with concept-level matches (hybrid).

## When to use

Use semantic to search by code meaning, not just keywords. Semantic search
understands concepts and finds related code even when exact terms don’t match.

Use hybrid when:

- You need both concept AND keywords present
- Semantic search returns too many false positives
- Keyword search misses related code
- You want balanced precision and recall

### Grep Replacement

```shell
# Pattern search
ck "pattern" file.txt
ck "pattern" *.rs
ck "TODO|FIXME" src/

# Case-insensitive
ck -i "warning" src/

# Whole word match
ck -w "test" src/

# Invert match
ck -v "exclude" src/
Output Control

# Line numbers
ck -n "pattern" file.txt

# Only filenames with matches
ck -l "pattern" src/

# Only filenames without matches
ck -L "pattern" src/

# Hide filenames
ck --no-filename "pattern" file1 file2

# Count matches
ck -c "pattern" src/
Context Display

# N lines after match
ck -A 3 "error" src/

# N lines before match
ck -B 2 "error" src/

# N lines before and after
ck -C 2 "error" src/

# Context with line numbers
ck -n -C 3 "error" src/
Recursive Search

# Recursive
ck -R "pattern" .

# Alternative
ck -r "pattern" .

# With exclusions
ck -R --exclude "*.test.js" "pattern" src/
File Filtering

# Specific file types
ck "pattern" **/*.rs
ck "pattern" **/*.{js,ts}

# Exclude patterns
ck --exclude "*.test.js" "pattern" src/
ck --exclude "node_modules" "pattern" .

# Multiple exclusions
ck --exclude "dist" --exclude "build" "pattern" .
```

### Semantic

Find code by meaning, not just keywords. Semantic search understands concepts
and finds related code even when exact terms don’t match.

Examples:

```shell
# Find error handling patterns
ck --sem "error handling" src/

# Find authentication code
ck --sem "user authentication" src/

# Find database operations
ck --sem "SQL queries" src/
```

With relevance scores:

```shell
# Show how relevant each match is
ck --sem --scores "caching" src/

# Output:
# [0.847] ./cache_manager.rs: LRU cache implementation
# [0.732] ./http_client.rs: Response caching with TTL
# [0.651] ./config.rs: Configuration caching
```

Scores range from 0.0 to 1.0, with higher scores indicating stronger semantic
similarity.

### Hybrid Search

Use hybrid when:

- You need both concept AND keywords present
- Semantic search returns too many false positives
- Keyword search misses related code
- You want balanced precision and recall

Hybrid search merges semantic and keyword search results to give you the best of
both worlds:

- Semantic – Finds conceptually related code
- Keyword – Ensures query terms are present
- Fusion – Ranks results by combined relevance

Simple Examples:

```shell
# Simple hybrid search
ck --hybrid "connection timeout" src/

# With relevance scores
ck --hybrid --scores "cache invalidation" src/

# Filter by confidence (note: RRF scores are typically 0.01-0.05)
ck --hybrid --threshold 0.02 "auth" src/
```

Due to Reciprocal Rank Fusion (RRF) mathematics, hybrid scores typically range
from 0.01 to 0.05, not 0.0 to 1.0 like semantic search. This is a known
counterintuitive aspect of RRF scoring.

Threshold Filtering Examples:

```shell
# High confidence only (RRF scale)
ck --hybrid --threshold 0.025 "pattern" src/

# Exploratory (lower confidence, more results)
ck --hybrid --threshold 0.015 "pattern" src/

# Very strict (only top matches)
ck --hybrid --threshold 0.03 "pattern" src/
```

Choosing the Right Threshold:

> Start with --threshold 0.02 for hybrid search. If you get too many results,
> increase to 0.025 or 0.03. If too few, decrease to 0.015. Use --scores to see
> actual RRF values and calibrate your threshold accordingly.

Result limiting example:

```shell
# Top 10 results
ck --hybrid --topk 10 "pattern" src/

# With threshold
ck --hybrid --topk 20 --threshold 0.02 "pattern" src/
```
