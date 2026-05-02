> Status: R&D / scratchpad. Part of Sage / 0SxD's prompt-engineering research portfolio. Content may move, change, or be withdrawn. See LICENSE for terms.

---
title: "research-wiki"
owner: Sage
github: 0SxD
license: MIT
date: 2026-04-19
pattern_source: "Andrej Karpathy, LLM Wiki (https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)"
---

# research-wiki

A personal research knowledge base built on the LLM Wiki pattern described by Andrej Karpathy: raw sources held immutable, an LLM-maintained wiki layer compiled from them, and a schema file that disciplines how the agent ingests, queries, and lints the corpus. Knowledge is compiled once and kept current, not re-derived on every query.

This repository is the public surface of that process. It is a working wiki that happens to be visible.

---

## Layout

| Path | Role |
|---|---|
| [`raw/`](./raw) | Immutable source documents. The agent reads from `raw/` but never writes back into it. |
| [`wiki/`](./wiki) | LLM-owned knowledge base: entity pages, concept pages, syntheses. Entry points are `wiki/index.md` (content-oriented catalog) and `wiki/log.md` (chronological, append-only). |

`raw/` and `wiki/` seed empty to keep the skeleton honest; they grow as the wiki is maintained.

---

## The three layers (per the source pattern)

1. **Raw sources** are immutable. Articles, papers, transcripts, images, data files. The wiki is derived from them; they are never rewritten.
2. **Wiki** is LLM-owned. It is a directory of interlinked markdown. When a new source arrives, the agent reads it, extracts the key information, writes or updates summary pages, touches cross-references across the wiki, and appends an entry to `wiki/log.md`. The human reviews and redirects; the human does not hand-maintain the wiki.
3. **Schema** is a single file (here, `AGENTS.md`) that defines conventions, workflows, and guardrails. It co-evolves with the wiki.

---

## Operations

- **Ingest.** Drop a source into `raw/`. Ask the agent to process it. The agent reads, summarizes, updates entity and concept pages, updates `wiki/index.md`, and appends to `wiki/log.md`.
- **Query.** Ask a question against the wiki. The agent reads `wiki/index.md` first, drills into relevant pages, synthesizes an answer with citations, and files the synthesis back into the wiki as a new page when the answer is worth keeping.
- **Lint.** Periodically ask the agent to health-check: contradictions, stale claims, orphans, missing cross-references, gaps. The wiki is a living artifact, not an archive.

Log entry format: `## [YYYY-MM-DD] <op> | <title>`. Parseable with `grep "^## \[" wiki/log.md | tail -N`.

---

## Why this layout

Most people experience LLMs-plus-documents as RAG: fragments retrieved and re-synthesized every query. Nothing accumulates. The wiki pattern is the opposite. The synthesis is compiled once, cross-referenced across pages, and kept current by the agent as new sources arrive. The cost of accumulation approaches zero; the value compounds.

The human's job is curation, direction, and good questions. The agent's job is everything else.

---

## Attribution

Pattern adapted from Andrej Karpathy's public "LLM Wiki" gist (2026). Adaptation and all original content under MIT (Sage / 0SxD, 2026). Upstream acknowledgements in [`ATTRIBUTION.md`](./ATTRIBUTION.md).
