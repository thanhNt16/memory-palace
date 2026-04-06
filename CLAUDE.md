# Memory Palace — LLM Wiki Schema

## Overview

This is a personal knowledge base maintained by Claude. Raw sources go into `raw/`. The LLM compiles them into a structured, interlinked wiki in `wiki/`. You curate sources and ask questions. Claude does the bookkeeping.

## Directory Structure

```
memory-palace/
├── CLAUDE.md          # This file — schema and conventions
├── raw/               # Immutable source documents (read-only)
│   └── assets/        # Downloaded images
├── wiki/              # LLM-owned markdown wiki
│   ├── index.md       # Content catalog — updated on every ingest
│   └── log.md         # Append-only chronological log
└── .obsidian/         # Obsidian vault config
```

## Wiki Page Format

Every page in `wiki/` uses YAML frontmatter:

```yaml
---
title: Page Title
tags: [concept, entity, source, analysis, meta]
sources: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

- One concept or entity per page.
- Use `[[wikilinks]]` for internal cross-references.
- Keep pages focused and factual.

## Special Files

### wiki/index.md
Content catalog. Lists every wiki page organized by category with a one-line summary and link. Claude updates this on every ingest and when new pages are created during queries.

### wiki/log.md
Append-only log. Entry format:

```
## [YYYY-MM-DD] operation | Description
```

Operations: `ingest`, `query`, `lint`, `maintenance`

Parseable with: `grep "^## \[" wiki/log.md | tail -5`

## Operations

### Ingest (`ingest`)
When the user provides a new source in `raw/`:

1. Read the source document
2. Discuss key takeaways with the user
3. Create a source summary page in `wiki/`
4. Create or update entity and concept pages
5. Add cross-references (`[[wikilinks]]`) between related pages
6. Update `wiki/index.md`
7. Append entry to `wiki/log.md`

A single source may touch 10-15 wiki pages. Stay involved with the user — check emphasis, correct misunderstandings.

### Query (`query`)
When the user asks a question:

1. Read `wiki/index.md` to find relevant pages
2. Read relevant wiki pages for context
3. Synthesize an answer with `[[citations]]` to wiki pages
4. If the answer is substantial, offer to file it as a new wiki page
5. If filed, update index and log

### Lint (`lint`)
When the user asks for a health check:

1. Scan for contradictions between pages (flag with ⚠️)
2. Find stale claims superseded by newer sources
3. Identify orphan pages with no inbound links
4. Find important concepts mentioned but lacking their own page
5. Find missing cross-references
6. Suggest data gaps that could be filled with web search
7. Suggest new questions to investigate

### Maintenance (`maintenance`)
When updating the wiki structure or conventions:

1. Document what changed and why
2. Update CLAUDE.md if conventions evolve
3. Append entry to log

## Search

The wiki can be searched using `qmd`:

```bash
qmd search "query" --dir wiki/
```

Use this when the index alone isn't sufficient for finding relevant pages.

## Style Rules

- Clear, neutral prose. No filler or hedging.
- Specific claims over vague generalizations.
- Always cite sources: "According to [[source-page]]..."
- Flag contradictions explicitly: "⚠️ Contradicts [[page]] which claims..."
- Use headers (`##`, `###`) for structure within pages.
- Use bullet points for lists, tables for comparisons.
