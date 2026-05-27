# X Bookmark Knowledge Graph — 1,962 Bookmarks → Synthesized Graph

A daily pipeline that pulls my X (Twitter) bookmarks and synthesizes each one into a structured graph organized by theme, project, and destination. Built to validate the architecture for the kind of public-signal intelligence layer a GTM team would want, using my own bookmark corpus as the working dataset.

**Scale:** 1,962 bookmarks captured, 25+ themes, full graph with synthesis + indexes
**Stack:** Camoufox (persistent browser profile) + Python + Claude for synthesis + JSON indexes

## Why I built it

X bookmarks are an interesting test corpus. Two years of saves, ~2,000 items, no useful query layer in X's UI. The corpus has the same shape as the signal layer most GTM teams want to build over LinkedIn, Apollo, or intent data: heterogeneous, unstructured, accumulating daily, hard to query.

I wanted to design the architecture against a corpus I owned, where I controlled the inputs and could iterate on the synthesis schema without contractual or rate-limit constraints. The bookmark corpus was that.

## How it works

**Capture layer.** Camoufox-based scraper that uses my logged-in X session (persistent browser profile, no cookie rotation needed). Pulls every bookmark, deduplicates against the existing dataset. Designed to run on a daily cron; the current run cadence depends on whether the X session is fresh.

**Per-bookmark synthesis.** For each new bookmark, the pipeline writes three files:

```
05_datasets/bookmarks/knowledge/items/<bookmark-id>/
  source.json       # raw tweet + metadata
  content.md        # tweet text + linked-asset extracts (articles, threads)
  synthesis.json    # Claude-extracted themes, projects, destinations, why-it-matters
```

**Indexes for query.** Top-level JSON indexes that downstream tools read:

```
indexes/
  bookmarks-by-id.json      # canonical lookup
  bookmarks-by-url.json     # dedup + cross-reference
  synthesis-themes.json     # theme → bookmark IDs
  synthesis-destinations.json   # where each bookmark should route
  bookmark-corpus.json      # the whole graph
```

**Incremental backfill.** New scrapes append to the indexes. Synthesis is idempotent — if the source tweet didn't change, the synthesis is skipped.

## What the graph supports

The architecture is built for these query patterns — these are the use cases the indexes are designed to answer, not a daily personal workflow:

- **Theme query:** retrieve all bookmark IDs tagged with a given theme (e.g., `ai-agents`)
- **Destination routing:** for each bookmark, which downstream surface should consume it (a content pipeline, a research note, a project doc)
- **Cross-reference deduplication:** the URL index catches the same article saved twice with different X status IDs
- **Corpus snapshot:** the `bookmark-corpus.json` is the full graph state, suitable for batch reads by downstream tools

The graph is in place; the daily backfill is running. I haven't built a personal UI on top of it because the goal was to validate the build pattern, not to replace my X bookmarks UX. Most of the leverage is in the synthesis schema and the index shapes, both of which are reusable across data sources.

## What I learned

- **The synthesis schema is the moat.** Anyone can scrape bookmarks. The actual value is in the YAML frontmatter on each `content.md` — themes, projects, destinations, why-it-matters. That's the part Claude is doing well that humans don't.
- **Daily backfill keeps it honest.** Big "one-time" scrapes go stale. A small daily run that adds 5-20 new items and re-synthesizes anything updated keeps the graph fresh without ever doing a heavy lift.
- **The bookmarks themselves are biased toward what I save when I'm low-effort.** That's actually useful — the graph reflects my real interests, not what I'd say my interests are.
- **Camoufox + persistent profile is the right pattern.** Cookie-jar approaches break every few weeks. A persistent browser profile with humanized launch options has been stable for months.

## Why this maps to GTM work

This is the exact pattern a sales or marketing team would build for lead-signal intelligence. Replace "X bookmarks" with "LinkedIn activity," "Apollo contacts," or "intent data," and the pipeline shape is identical: scrape → synthesize → index → query.

The architecture transfers directly:
- Capture layer (Camoufox + persistent session) → any authenticated source
- Synthesis schema (Claude with a JSON output contract) → any unstructured signal
- Index layer (by-id, by-url, by-theme, by-destination) → any retrieval pattern a GTM team needs

The skill is in the synthesis schema and the indexing strategy, not the capture. I built this for myself; the same architecture is what I'd ship on day one for a GTM team that wants to turn unstructured public signals into a queryable graph.
