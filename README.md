# Your AI Maintains the Wiki, You Just Feed It

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Markdown](https://img.shields.io/badge/Markdown-Native-083fa1.svg)](https://commonmark.org/)
[![Karpathy LLM Wiki](https://img.shields.io/badge/Pattern-Karpathy_LLM_Wiki-purple.svg)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

[正體中文](README_zh.md)

![LLM Wiki v0.1 hero](docs/images/hero.png)

You know that knowledge base you "started organizing properly" three times last year? Yeah. This one's different — because the one maintaining it isn't you.

Production-ready template based on [Andrej Karpathy's 2026-04 LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Three-tier architecture (raw / wiki / schema) plus a journal layer extension. Clone it, feed it docs, let the LLM do the bookkeeping humans always abandon.

## Why not just use Obsidian / Notion / Confluence

Those tools assume **you** maintain the knowledge base. Cross-references, summaries, contradictions, missing pages — somebody has to keep it consistent. Humans give up around page 50.

LLMs don't give up. This template is the structure that lets them help.

- **LLM maintenance cost ≈ zero**: no fatigue from updating cross-refs, no skipping bookkeeping
- **Three tiers, clean separation**: raw = source of truth, wiki = LLM's organized output, schema = the rules
- **Git native**: version control, diff, optional git-crypt encryption
- **No vendor lock-in**: pure markdown — switch LLMs or tools tomorrow, take your wiki with you

## Quick Start

```bash
git clone https://github.com/YOUR/kc_llm_wiki_starter.git my-wiki
cd my-wiki
```

1. Replace `{{ your-wiki-name }}` placeholders in `CLAUDE.md` / `README.md` / `index.md` / `log.md`
2. Read `wiki/_example_entity.md` and `wiki/_example_overview.md` to learn the frontmatter convention
3. Delete examples (`git rm wiki/_example_*.md`) or keep them as reference
4. Open a Claude session, drop a doc into `raw/`, and say "ingest this into wiki"

Full walkthrough: [docs/getting-started.md](docs/getting-started.md)

## Core Concepts (30-second version)

```
raw/     ← External dumps (gitignored, not in git)
wiki/    ← LLM-maintained topic pages (frontmatter + body)
journal/ ← Temporal work logs (daily / meeting / checklist)
SCHEMA.md, log.md, index.md  ← meta layer
```

**Three workflows**:

1. **Ingest**: feed raw doc → agent confirms key points with you → writes to wiki → updates index + log
2. **Query**: ask agent → reads index → reads wiki → answers with citations
3. **Lint**: run `/llm-wiki-lint` periodically → catches stale claims, orphan pages, missing topics, data gaps

Detailed: [docs/workflow.md](docs/workflow.md)

## Companion skill

[`kc_ai_skills/llm-wiki-lint`](https://github.com/KerberosClaw/kc_ai_skills/tree/main/llm-wiki-lint) — because once your wiki passes ~15 pages, stale claims, broken cross-refs, and missing topics start rotting silently. Humans don't catch this. The lint skill does.

This template's `CLAUDE.md` already includes a session-start hygiene check — your agent will remind you to run lint when it's been more than 14 days since the last one.

**Install**:

```bash
git clone https://github.com/KerberosClaw/kc_ai_skills.git
cp -r kc_ai_skills/llm-wiki-lint ~/.claude/skills/
```

## Security Notice

This template ships with **no actual data, credentials, or external dependencies** — it's just markdown scaffolding. That said:

- **Your `raw/` folder is gitignored** by design. But anything you put there lives on your local disk — plan your own backup / sync strategy.
- **Commit messages are NOT encrypted by git-crypt**, even if you enable it. Keep sensitive entity names out of commit messages.
- **External LLM APIs**: when your agent ingests / queries, content is sent to whichever LLM you use (Claude, GPT, etc.). Apply your own data classification policy before feeding sensitive material.
- **Report security issues**: open a GitHub issue (this template repo is intended for non-sensitive content).

## Documentation

- [docs/getting-started.md](docs/getting-started.md) — 5-minute first ingest
- [docs/workflow.md](docs/workflow.md) — Ingest / Query / Lint deep dive
- [docs/extensions.md](docs/extensions.md) — Customization (journal layer, frontmatter extensions, tool integrations)

## License

MIT — see [LICENSE](LICENSE).
