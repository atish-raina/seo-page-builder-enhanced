# SEO Page Builder (enhanced)

A Claude Code skill for building deeply-researched SEO pages: keyword research, SERP analysis,
primary-source mining, fact verification, writing, site integration, and QA.

This is an extension of **Octolens' SEO Page Builder skill**. The original pipeline (real search data
decides what to build, real user quotes make it hard to copy) is Octolens'. This fork keeps that
methodology and adds a layer the original does not cover: what the finished page has to *read* like.

## What this fork adds

The original skill answers "what goes on the page". A well-researched draft can still read like machine
output, and a page that reads like machine output does not get cited, linked or trusted, whatever its
schema says. So this version adds:

- **A house style file ([STYLE.md](STYLE.md))**, enforced at three points in the pipeline: while writing
  (Phase 4), during the human pass (Phase 5, move 3), and at QA (Phase 7). It covers four things: stripping
  the machine fingerprints (em dashes as default connectors, the word blacklist, templated constructions),
  a measured Flesch-Kincaid target of grade 8 to 10 with the scoring script included, a no-clickbait rule,
  and treating bullets as a data structure rather than a formatting choice.
- **Phase 5, the human pass**, promoted to a named phase with five ordered moves: inject firsthand
  experience, add what the model could not know, cut filler and enforce style, fact-check and refresh, put a
  real author on it. Budget 20 minutes minimum, an hour or more for a deep page.
- **Two extra hard rules.** One generation pass, then human edits, because each model rewrite regresses the
  text toward the average of its training distribution. And a named human owns the byline, which is an
  E-E-A-T requirement and an accountability one.
- **A note that search and social are different products.** The Phase 5 moves rescue a draft for search.
  They do not transfer to social, where audiences prefer human-written posts and several platforms
  down-rank machine-generated ones. For social, invert the split.

## Install

Claude Code reads skills from `~/.claude/skills/` (user level) or `.claude/skills/` (project level).
`SKILL.md` expects the style guide at `references/STYLE.md`, so the layout matters:

```bash
mkdir -p ~/.claude/skills/seo-page-builder/references
git clone https://github.com/atish-raina/seo-page-builder-enhanced.git /tmp/spb
cp /tmp/spb/SKILL.md ~/.claude/skills/seo-page-builder/
cp /tmp/spb/STYLE.md ~/.claude/skills/seo-page-builder/references/
```

Then ask Claude to build any SEO-targeted page: an alternatives roundup, a "best X tools" listicle, a
comparison page, a pricing guide, a how-to, or a glossary entry. The skill triggers on those requests.

## Data sources

Everything is optional and the skill degrades gracefully, but the output is only as good as the data
behind it.

| Need | Options | Env var |
|---|---|---|
| Keyword volume, difficulty, CPC | Ahrefs API v3, DataForSEO, Semrush | `AHREFS_API_KEY`, `DATAFORSEO_LOGIN` / `DATAFORSEO_PASSWORD` |
| Live SERP snapshots | DataForSEO SERP API, Ahrefs SERP overview, manual incognito search | same as above |
| Social / primary-source data | Octolens Search API, or manual Reddit / Hacker News / X / G2 search | `OCTOLENS_API_KEY` |
| Fact verification | WebFetch / WebSearch against official pricing and docs | none |

With no SEO API configured, the skill asks you for volumes and SERP screenshots instead of guessing
numbers. With no social source, it mines manually. Slower, same standards.

## The pipeline

| Phase | What happens |
|---|---|
| 0 | Scope the page: topic, page type, new or refresh |
| 1 | Keyword research with real volumes, plus a live SERP read and a cannibalization check |
| 2 | Primary-source mining: 6 to 10 verbatim, linkable quotes from real users |
| 3 | Fact verification against official pricing and docs, dated today |
| 4 | Write, in the register defined by STYLE.md |
| 5 | The human pass (the step that cannot be delegated) |
| 6 | Site integration: sitemap, internal links, redirects, assets, meta |
| 7 | QA, then hand off as a draft or PR |

Hard rules the skill will not bend on: unique content per page, verbatim quotes with working links, facts
verified at write time, honest treatment of competitors, drafts rather than auto-publish, and a refusal to
mass-generate. One deep page per run.

## Credit

The methodology and the original skill are Octolens'. This repo is an extension of it, not a replacement.
Bugs and style opinions in this fork are mine.

## License

MIT. See [LICENSE](LICENSE).
