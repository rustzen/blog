# AI Signals Commit Automation

This document is the single source of truth for the `AI Signals Commit` scheduled task.

The ChatGPT automation prompt should only fetch this file from GitHub and execute the rules below. Do not duplicate these rules in the automation prompt.

## Target repository

- Repository: `idaibin/blog` / Rustzen blog
- Production branch: `main`
- Generated content directory: `src/content/signals/`
- Pull requests: do not create PRs for scheduled runs
- Commit target: commit generated signal posts directly to `main` when there is enough meaningful source-backed content

## Task objective

Search for the latest AI news, product releases, research, GitHub projects, and emerging AI concepts from the previous day through the current run time.

Produce a daily AI signals update for the Rustzen blog when at least 3 meaningful source-backed items are available.

## Source priority

Prioritize source-backed signals from:

1. Primary sources: company blogs, official product announcements, research papers, arXiv, GitHub repositories, standards bodies, official documentation.
2. Reputable technology and business outlets when primary sources are unavailable or when they add useful market context.
3. Avoid unsupported social-media-only claims unless they link to verifiable primary material.

Clearly distinguish:

- confirmed product releases
- research papers
- GitHub projects
- new concepts or techniques
- speculative trends

Do not present speculation as confirmed news.

## Output files

For the current run date, create or update both files:

- `src/content/signals/YYYY-MM-DD.zh.mdx`
- `src/content/signals/YYYY-MM-DD.en.mdx`

Use the run date in Asia/Shanghai timezone unless the execution environment explicitly requires another timezone.

## Frontmatter format

Use this frontmatter style:

```yaml
title: YYYY-MM-DD AI Signals
description: One short sentence summarizing the daily AI update.
pubDate: YYYY-MM-DD
tags: ["AI", "Signals"]
audience: ["developer", "ai-practitioner"]
featured: true
```

Rules:

- `title` must be `YYYY-MM-DD AI Signals`.
- `description` must be one short sentence.
- Do not use wording like `信息流`, `feed`, `覆盖时间`, or `coverage window` in the description.
- `tags` should be compact. Add only useful topical tags.

## Body format

Do not add a coverage window line.

Do not add section headings such as:

- `## 信息流`
- `## Feed`
- `Key Conclusions`
- `Selected Signals`
- `Signal Technique`
- `Observations`
- `Promotion Candidates`

Each item must follow this pattern:

```mdx
### <a href="https://example.com/source" target="_blank" rel="noopener noreferrer">Title ↗</a>

Source Name · YYYY-MM-DD · type / topic

One short paragraph describing what happened.

**值得关注**：One short Chinese sentence explaining why it matters.
```

For English:

```mdx
### <a href="https://example.com/source" target="_blank" rel="noopener noreferrer">Title ↗</a>

Source Name · YYYY-MM-DD · type / topic

One short paragraph describing what happened.

**Why it matters**: One short English sentence explaining why it matters.
```

Separate items with:

```mdx
---
```

## Link rules

For every item:

- The heading must be an HTML anchor.
- The heading text itself must link to the original source.
- The title must end with the external-link marker `↗`.
- The link must include `target="_blank"`.
- The link must include `rel="noopener noreferrer"`.
- Do not use bare Markdown links as the item heading.

## Quality threshold

If fewer than 3 meaningful source-backed items are found:

- Do not create or update the daily MDX post.
- Do not commit a low-value post.
- Report that there were not enough meaningful updates.
- List the sources checked.

## Validation before commit

Before committing, verify:

- Frontmatter is valid.
- MDX has no broken Markdown tables.
- MDX has no bare unescaped JSX issues.
- Source links use the required HTML anchor style.
- Both Chinese and English files exist and are aligned.
- Claims are source-backed and not overstated.

## Commit rule

When a post is generated, commit with this message format:

```text
chore(signals): add AI signals for YYYY-MM-DD
```

Do not create a pull request for scheduled runs.
