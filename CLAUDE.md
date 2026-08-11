# CLAUDE.md

Instructions for Claude Code when working in this repository.

## What this repo is

A public GitHub repo (`humanpen/ai-humanizer-comparison`) containing an SEO comparison article — "What Is HumanPen?" — published as multilingual GitHub READMEs. It ranks HumanPen against 10 AI humanizer competitors across document workflow, input limits, formatting preservation, and Turnitin integration.

This is a **content-only** repo. There is no build system, no tests, no dependencies. Every change is a text edit to one or more Markdown files.

## Repo structure

```
README.md          # English (canonical)
README.zh-CN.md    # Chinese
README.ko.md       # Korean
README.ar.md       # Arabic
README.vi.md       # Vietnamese
README.ja.md       # Japanese
README.es.md       # Spanish
README.pt-BR.md    # Brazilian Portuguese
assets/            # Images referenced by the articles
```

All 8 READMEs share the same section structure and the same official-sources list. The first line of every README is the language switcher linking to all other versions.

## Content authority

- **Competitor facts** come from `doc/seo/landing-pages/01-competitor-field-research.md` in the `auto_deep_polish` repo. That document is the single source of truth for pricing, limits, features, and competitor claims. Do not invent competitor facts; if the research doc does not cover something, say so or leave it out.
- **Verification date**: every claim traces to a dated survey. The current date is recorded in the main product codebase (`COMPETITOR_DATA_VERIFIED_ON` in `server/web/lib/compare-targets.ts`). When updating competitor facts here, verify the research doc is also current.

## Relationship with the main product

The same article content also appears in two other places inside `auto_deep_polish`:

| Location | Format | Notes |
|---|---|---|
| `server/web/messages/en.json` / `zh-CN.json` | Blog JSON blocks under `Blog.articles.ai-humanizer-comparison-2026` | Rendered on humanpen.net; supports `[text](url)` inline links, `**bold**`, `*italic*` |
| `doc/seo/landing-pages/content-hub/ai-humanizer-comparison-2026.{en,}.md` | Markdown source files | EN and ZH only; used as content-hub drafts |

When updating content here, check whether the same change applies to those locations too. The three copies are not auto-synced.

## Git identity and push rules

This repo belongs to the `humanpen` GitHub org. All pushes must use the **yotta-fish** SSH identity:

- Remote is already configured as `git@github-yotta:humanpen/ai-humanizer-comparison.git`
- The SSH alias `github-yotta` maps to `~/.ssh/id_ed25519_yotta_fish`
- **Never** expose `yuhuang-cst` or any other identity in commits or pushes to this org

Verify with `git remote -v` before pushing.

## SOP: Updating content

1. **Identify what changed** — a new HumanPen feature, a competitor pricing change, a factual correction, etc.
2. **Update the English README first** (`README.md`). It is the canonical version.
3. **Propagate to all 7 other language READMEs** — same section, same position. Keep each translation natural in its language; do not machine-translate a sentence and paste it.
4. **Keep the three parallel locations consistent** — if the change also affects the blog article (`en.json` / `zh-CN.json`) or the content-hub markdown, update those in `auto_deep_polish` as well.
5. **Commit and push**:
   - One commit per logical change
   - Commit message: concise, imperative, English. Example: `Add humanpen-skill and humanpen-mcp GitHub links`
   - Push to `origin main`

## SOP: Adding a new language

1. Copy `README.md` to `README.<locale>.md`
2. Translate fully — do not leave English fragments
3. Add the new file to the language switcher line at the top of **every** existing README
4. Commit all files together in one commit

## SOP: Adding a new competitor

1. Confirm the competitor is documented in the research doc (`01-competitor-field-research.md`)
2. Add a row to the comparison table in every README
3. Add an entry to the "Official sources" section in every README
4. Check whether a `/compare/<slug>` page should also be created in the main product (in `compare-targets.ts`, `en.json`, `zh-CN.json`, `sitemap-routes.json`)

## Style rules

- **Markdown only** — no HTML tags in the READMEs
- **No emoji** unless the user explicitly requests it
- Links use standard Markdown `[text](url)` syntax
- Product names use their official capitalization (HumanPen, Undetectable.ai, WriteHuman, StealthGPT, HIX Bypass, QuillBot, etc.)
- Every README ends with a trailing newline
- Sections within a language version follow the same order as the English canonical

## What NOT to do

- Do not add CI, linting, or build tooling — this is a pure content repo
- Do not commit images larger than 500 KB without discussing first
- Do not claim HumanPen is "more effective" at reducing AI scores — the research doc explicitly forbids this
- Do not fabricate competitor data; if unsure, check the research doc or leave it out
- Do not push with any identity other than yotta-fish
