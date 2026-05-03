# LLM Wiki Schema

This document defines the structure, conventions, and workflows for maintaining this LLM Wiki knowledge base.

## Directory Structure

```
llm-wiki/
├── raw/                    # Immutable source documents
│   └── assets/            # Downloaded images, attachments
├── wiki/                  # LLM-generated markdown pages
│   ├── concepts/         # Concept pages
│   ├── entities/         # Entity pages
│   ├── sources/          # Source summaries
│   └── syntheses/        # Synthesis pages
├── CLAUDE.md              # This schema file
├── index.md               # Content catalog of all wiki pages
└── log.md                 # Chronological record of operations
```

## Three Layers

### 1. Raw Sources (`raw/`)
- **Purpose**: Immutable collection of source documents
- **Contents**: Articles, papers, images, data files
- **Rule**: LLM reads from here but NEVER modifies
- **File types**: .md, .txt, .pdf, .png, .jpg, etc.

### 2. Wiki (`wiki/`)
- **Purpose**: LLM-generated and maintained knowledge base
- **Contents**: Source summaries, entity pages, concept pages, syntheses
- **Rule**: LLM owns this layer entirely — creates, updates, maintains
- **User role**: Read and browse, never edit directly

### 3. Schema (`CLAUDE.md`)
- **Purpose**: Configuration file that instructs the LLM
- **Contents**: Structure, conventions, workflows
- **Rule**: Co-evolved by user and LLM over time

## Wiki Page Types

### Source Summaries
- **Location**: `wiki/sources/[source-name].md`
- **Purpose**: Digest of a single raw source
- **Format**:
  ```markdown
  ---
  type: source
  source: [path to raw file]
  date_ingested: [YYYY-MM-DD]
  tags: [tag1, tag2, ...]
  ---

  # [Source Title]

  **Source**: [link to raw file]
  **Date**: [date]
  **Type**: [article/paper/book/etc]

  ## Key Takeaways
  - [point 1]
  - [point 2]

  ## Summary
  [2-4 paragraph summary]

  ## Entities Mentioned
  - [[Entity Name]] — [context]
  - [[Entity Name]] — [context]

  ## Concepts
  - [[Concept Name]] — [context]
  ```

### Entity Pages
- **Location**: `wiki/entities/[entity-name].md`
- **Purpose**: Central page for a person, organization, place, character, etc.
- **Format**:
  ```markdown
  ---
  type: entity
  created: [YYYY-MM-DD]
  last_updated: [YYYY-MM-DD]
  source_count: [number]
  tags: [tag1, tag2, ...]
  ---

  # [Entity Name]

  ## Overview
  [1-2 paragraph description]

  ## Key Information
  - **Type**: [person/organization/place/character/etc]
  - **First mentioned**: [source]
  - **Related entities**: [[Entity1]], [[Entity2]]

  ## Appearances in Sources
  - [Source Title] — [context]
  - [Source Title] — [context]

  ## Connections
  - Connected to [[Entity]] via [relationship]
  - Connected to [[Concept]] via [relationship]
  ```

### Concept Pages
- **Location**: `wiki/concepts/[concept-name].md`
- **Purpose**: Central page for an idea, theory, theme, pattern
- **Format**:
  ```markdown
  ---
  type: concept
  created: [YYYY-MM-DD]
  last_updated: [YYYY-MM-DD]
  source_count: [number]
  tags: [tag1, tag2, ...]
  ---

  # [Concept Name]

  ## Definition
  [Clear definition]

  ## Key Aspects
  - [aspect 1]
  - [aspect 2]

  ## Sources Discussing This
  - [Source Title] — [how it discusses]
  - [Source Title] — [how it discusses]

  ## Related Concepts
  - [[Concept1]] — [relationship]
  - [[Concept2]] — [relationship]

  ## Open Questions
  - [question 1]
  - [question 2]
  ```

### Comparison Pages (Optional)
- **Location**: Create in appropriate subdirectory (e.g., `wiki/concepts/[comparison-name].md`)
- **Purpose**: Side-by-side analysis of entities, concepts, or sources
- **Note**: Comparisons are optional and can be created as needed
- **Format**:
  ```markdown
  ---
  type: comparison
  created: [YYYY-MM-DD]
  comparing: [[Entity1]], [[Entity2]]
  ---

  # [Comparison Title]

  ## Overview
  [Purpose of comparison]

  ## Comparison Table
  | Aspect | [Entity1] | [Entity2] |
  |--------|-----------|-----------|
  | [aspect] | [value] | [value] |
  ```

### Synthesis Pages
- **Location**: `wiki/syntheses/[topic].md`
- **Purpose**: High-level synthesis across multiple sources
- **Format**:
  ```markdown
  ---
  type: synthesis
  created: [YYYY-MM-DD]
  last_updated: [YYYY-MM-DD]
  scope: [broad topic area]
  ---

  # [Topic] Synthesis

  ## Thesis
  [Main argument or insight]

  ## Key Points
  - [point 1]
  - [point 2]

  ## Supporting Evidence
  - From [[Source]]: [evidence]
  - From [[Source]]: [evidence]

  ## Contradictions
  - [Contradiction between sources]
  - [Resolution or note]

  ## Open Questions
  - [question 1]
  ```

## Cross-Reference Convention

- Use `[[Page Name]]` syntax for internal links
- Always link to existing pages when mentioning entities/concepts
- Create new entity/concept pages when first mentioned if significant
- Link back from entity/concept pages to source pages

## Operations

### Ingest Workflow

When you add a new source:

1. **Read the source** from `raw/` directory
2. **Discuss key takeaways** with the user — confirm understanding
3. **Create source summary** in `wiki/sources/[name].md`
4. **Update index.md** — add the new source entry
5. **Update relevant entity pages** — create new ones if needed
6. **Update relevant concept pages** — create new ones if needed
7. **Update log.md** — append ingest entry with timestamp
8. **Report changes** — list all pages created/modified

**Log entry format**:
```markdown
## [YYYY-MM-DD] ingest | [Source Title]
- Created: wiki/sources/[name].md
- Updated: wiki/entities/[entity].md, wiki/concepts/[concept].md
- Summary: [1-2 sentence summary of what was added]
```

### Query Workflow

When answering a question:

1. **Read index.md** to find relevant pages
2. **Read relevant pages** to gather information
3. **Synthesize answer** with citations to wiki pages
4. **Offer to file the answer** as a new wiki page if valuable
5. **Update log.md** if a new page is created

**Log entry format**:
```markdown
## [YYYY-MM-DD] query | [Question summary]
- Read: [list of pages consulted]
- Created: wiki/[type]/[name].md (if applicable)
- Summary: [1-2 sentence summary]
```

### Lint Workflow

Periodically health-check the wiki:

1. **Check for contradictions** between pages
2. **Identify stale claims** superseded by newer sources
3. **Find orphan pages** with no inbound links
4. **Spot important concepts** mentioned but lacking their own page
5. **Note missing cross-references**
6. **Suggest data gaps** that could be filled with web search
7. **Suggest new questions** to investigate
8. **Update log.md** with findings

**Log entry format**:
```markdown
## [YYYY-MM-DD] lint | Wiki health check
- Contradictions found: [count]
- Orphan pages: [list]
- Missing pages: [list]
- Suggestions: [list of suggestions]
```

## Index File (`index.md`)

The index is a content catalog organized by category:

```markdown
# LLM Wiki Index

Last updated: [YYYY-MM-DD]

## Sources ([count])
- [[Source Title]] — [one-line summary] ([date])
- [[Source Title]] — [one-line summary] ([date])

## Entities ([count])
- [[Entity Name]] — [one-line description]
- [[Entity Name]] — [one-line description]

## Concepts ([count])
- [[Concept Name]] — [one-line definition]
- [[Concept Name]] — [one-line definition]

## Syntheses ([count])
- [[Topic Synthesis]] — [scope]
- [[Topic Synthesis]] — [scope]
```

**Rule**: Update index.md on every ingest and when creating new pages.

## Log File (`log.md`)

The log is an append-only chronological record:

```markdown
# LLM Wiki Log

## [2026-05-03] initialize | Wiki setup
- Created directory structure
- Created CLAUDE.md schema
- Created index.md
- Created log.md
```

**Rule**: Every operation (ingest, query, lint) gets a log entry with timestamp.

## YAML Frontmatter

All wiki pages should include YAML frontmatter:

```yaml
---
type: [source|entity|concept|synthesis]
created: [YYYY-MM-DD]
last_updated: [YYYY-MM-DD]
tags: [tag1, tag2, ...]
---
```

Additional fields by type:
- **source**: `source: [path]`, `date_ingested: [date]`
- **entity**: `source_count: [number]`
- **concept**: `source_count: [number]`
- **synthesis**: `scope: [topic area]`
- **comparison** (optional): `comparing: [[Entity1]], [[Entity2]]`

## Image Handling

- Images are stored in `raw/assets/`
- In wiki pages, reference images as: `![[raw/assets/filename.png]]`
- When processing a source with images:
  1. Read the text content first
  2. Then view referenced images separately for additional context
  3. Note any insights gained from images in the summary

## Linking Best Practices

- Always use `[[Page Name]]` for internal links
- Link to the most specific page available
- If an entity is mentioned in a source, link to its entity page
- If a concept is discussed, link to its concept page
- Bidirectional linking: ensure entity/concept pages link back to sources

## Consistency Rules

- **Naming**: Use consistent, descriptive page names
- **Dates**: Always use YYYY-MM-DD format
- **Updates**: Update `last_updated` field whenever a page is modified
- **Source counts**: Increment `source_count` on entity/concept pages when new sources reference them
- **Contradictions**: When new data contradicts old, note it explicitly on the relevant pages

## When to Create New Pages

Create a new entity page when:
- A person, organization, place, or character is mentioned in ≥2 sources
- The entity seems central to the topic
- The user asks about it specifically

Create a new concept page when:
- An idea, theory, or theme appears in ≥2 sources
- The concept seems important for understanding the domain
- The user asks about it specifically

Create a comparison page when:
- The user explicitly asks for a comparison
- A natural comparison emerges during analysis
- It would help clarify differences between entities/concepts

Create a synthesis page when:
- Multiple sources converge on a topic
- A high-level understanding is needed across sources
- The user asks for a broad overview

## User Interaction Principles

- **Stay involved**: Discuss key takeaways before writing
- **Confirm understanding**: Check that the LLM grasps the source correctly
- **Guide emphasis**: Let the user direct what to focus on
- **Review changes**: User should browse updates in Obsidian
- **Iterate**: Refine pages based on user feedback

## Tooling Notes

- **Obsidian**: The recommended IDE for browsing the wiki
- **Graph view**: Use to see connections and identify orphans
- **Git**: The wiki is a git repo — version history is automatic
- **Optional search**: At scale, consider qmd or similar for search
- **Dataview**: Can query frontmatter for dynamic views

## Evolution

This schema is a living document. As we work together:
- Note what conventions work well
- Add new page types as needed
- Refine workflows based on experience
- Update this file to reflect changes

The goal is a wiki that serves your specific needs — adapt as necessary.
