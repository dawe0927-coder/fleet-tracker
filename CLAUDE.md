# Knowledge Base Schema

A Karpathy-style external brain. Three layers; AI maintains the wiki, humans add to raw/.

## Layout

- `raw/` — original sources (read-only for AI). Subfolders: `articles/`, `papers/`, `repos/`, `datasets/`, `assets/`.
- `wiki/` — AI-generated. Subfolders: `concepts/`, `entities/`, `sources/`, `syntheses/`, `outputs/`, `attachments/`. Plus `index.md` (master index) and `log.md` (operation log).
- `templates/` — page skeletons for source/concept/entity/synthesis.
- `CLAUDE.md` — this file.

## Naming

- All filenames kebab-case. `active-inference.md` ✓ — `Active Inference.md` ✗.
- Source notes: `author-year-short-title.md` (e.g. `friston-2010-free-energy.md`).
- Wiki links: `[[page-slug]]`.

## Operations

### Ingest
For each new file in `raw/`:
1. Read it. Add a source page in `wiki/sources/` from `templates/source.md` with summary (200–500 words) and key points.
2. Extract concepts and entities. For each, either create a stub or update an existing page.
3. Add backlinks in both directions. Append a line to `wiki/log.md`.

### Compile
Rebuild `wiki/index.md` from current files. Promote stubs that now have ≥2 sources to full pages. Merge duplicates. Keep cross-links consistent.

### Query
Answer the question by reading `wiki/` only. Cite source pages inline as `[[source-slug]]`. Save the answer to `wiki/syntheses/` using `templates/synthesis.md`. Do not invent facts not in the wiki.

### Lint
Scan the wiki for: broken `[[links]]`, orphan pages, contradictions across sources, stale dates, duplicate concepts under different names. Flag with ⚠️ in `log.md`; auto-fix only mechanical issues (links, naming).

## Thresholds

- Concept appears in ≥2 sources → full page (500–1500 words). Otherwise → stub.
- Source summaries: 200–500 words.
- Synthesis answers: include ≥2 cited sources or state the gap explicitly.

## Rules

- AI never edits files under `raw/`.
- Humans don't hand-edit `wiki/` (re-run Compile instead).
- Every wiki page links to at least one source.
- When in doubt, prefer linking over duplicating prose.
