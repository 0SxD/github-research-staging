---
title: "research-wiki schema"
owner: Austin B. Green
type: schema
date: 2026-04-19
---

# research-wiki schema

The schema file tells any agent how this wiki is organized and what to do when ingesting, querying, or linting. Treat this file as the operating contract for the repository.

---

## Layout recap

```
raw/       immutable source documents (the agent reads, never writes)
wiki/      LLM-owned knowledge base (agent writes and maintains)
wiki/index.md   content-oriented catalog of everything in the wiki
wiki/log.md     chronological, append-only record of operations
```

---

## Ingest workflow

1. Read the source end to end before writing anything.
2. Update or create the appropriate entity pages, concept pages, and source summary page under `wiki/`.
3. Update `wiki/index.md` with the new or changed entries.
4. Append an entry to `wiki/log.md` using the format `## [YYYY-MM-DD] ingest | <source title>`.
5. Cross-link aggressively. A single ingest can touch 10-15 wiki pages.

Prefer updating an existing page over creating a new one when a concept is already present.

---

## Query workflow

1. Read `wiki/index.md` first to locate relevant pages.
2. Drill into the specific pages; do not attempt to load the entire wiki.
3. Synthesize an answer with inline citations to wiki pages and, where appropriate, back to `raw/`.
4. When the synthesis has standalone value, file it as a new wiki page and append `## [YYYY-MM-DD] query | <question>` to `wiki/log.md`.

---

## Lint workflow

Run periodically (every 10-15 ingests, or on demand):

- Contradictions between pages.
- Stale claims superseded by newer sources.
- Orphan pages with no inbound links.
- Concepts mentioned but lacking their own page.
- Missing cross-references.
- Gaps in coverage that warrant a new source.

Write findings to `wiki/log.md` with prefix `## [YYYY-MM-DD] lint | <scope>`.

---

## Page conventions

- Markdown with YAML frontmatter: `title`, `type`, `date`, optional `tags`, optional `sources`.
- Headings follow a consistent depth: `#` for page title, `##` for major sections.
- Inline links use relative paths. Backlinks are expected; keep them tight.
- Keep pages focused. If a page grows past roughly 800 words and covers more than one concept, split it.

---

## Out of scope

- Do not modify files under `raw/`.
- Do not invent sources. If a claim lacks a source, mark it `{{needs-source}}` and move on.

---

## License

MIT (Austin B. Green, 2026). Pattern derived from Andrej Karpathy's LLM Wiki gist.
