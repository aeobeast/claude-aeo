# /aeo-full — Complete AEO Audit

Run a comprehensive AEO/GEO audit of a URL. Fetch the page, robots.txt, llms.txt, and sitemap concurrently, then run all 5 specialist analyses in parallel.

## Steps

1. **Fetch in parallel** (all at once):
   - `GET {url}` — main page HTML + response headers
   - `GET {origin}/robots.txt`
   - `GET {origin}/llms.txt`
   - `HEAD {origin}/sitemap.xml` (check 200 vs 404)

2. **Run all 5 analyses in parallel** (spawn sub-agents):
   - AI Crawler Access → @agents/aeo-crawlers.md
   - Structured Data → @agents/aeo-schema.md
   - Content Citability → @agents/aeo-citability.md
   - Brand/Entity → @agents/aeo-entity.md
   - llms.txt → @agents/aeo-llmstxt.md

3. **Compute scores**

   Category scores (rule deduction method):
   - Start at 100 for each category
   - Deduct per issue: Critical = −15, Important = −8, Recommended = −3
   - Floor at 0

   Overall AEO Score = weighted average:
   | Category | Weight |
   |---|---|
   | AI Crawler Access | 20% |
   | Content Citability | 20% |
   | Structured Data | 15% |
   | E-E-A-T | 15% |
   | Entity Prominence | 15% |
   | llms.txt | 10% |
   | Technical | 5% |

   Per-platform scores (apply multipliers to category scores):
   | Platform | Boosted categories |
   |---|---|
   | ChatGPT | Crawlers ×1.5, Citability ×1.3, Entity ×1.2 |
   | Perplexity | Schema ×1.5, Citability ×1.4, llms.txt ×1.3 |
   | Claude | E-E-A-T ×1.5, Citability ×1.4, Crawlers ×1.3 |
   | Gemini | Schema ×1.4, E-E-A-T ×1.3, Entity ×1.3 |
   | Grok | Technical ×1.4, Crawlers ×1.3 |

4. **Output the full report** in this format:

---

## AEO Audit Report — {url}
*Audited: {date}*

### Overall AEO Score: {score}/100

| Platform | Score | Status |
|---|---|---|
| ChatGPT | {n} | {Not visible / Weak / Fair / Good / Excellent} |
| Perplexity | {n} | … |
| Claude | {n} | … |
| Gemini | {n} | … |
| Grok | {n} | … |

### Category Breakdown
| Category | Score | Issues |
|---|---|---|
| AI Crawler Access | {n}/100 | {count} issues |
| Content Citability | {n}/100 | … |
| Structured Data | {n}/100 | … |
| E-E-A-T | {n}/100 | … |
| Entity Prominence | {n}/100 | … |
| llms.txt | {n}/100 | … |
| Technical | {n}/100 | … |

### ⚡ Quick Wins (fix these first)
{List top 5 critical/important issues that are fast to fix — llms.txt, schema, robots.txt, noindex, canonical}

### Citability Analysis
- Citation-ready blocks found: {n} (target: 5+)
- Statistics detected in first 30% of content: {yes/no}
- Question-format headings: {n}/{total}
- External citations: {n}

### Full Issue List
{All issues sorted by score_impact desc, grouped by category. Format:}
**[CRITICAL/IMPORTANT/RECOMMENDED]** {title}
→ {recommendation}
→ Platforms: {platform_impact}
→ Score impact: +{n} pts if fixed

### Generated Assets
{If llms.txt is missing:}
**llms.txt — ready to publish at /llms.txt:**
```
{generated content}
```

{If Organization or FAQ schema is missing:}
**JSON-LD schema — add to <head>:**
```html
{generated schema}
```

---

*Track your AEO score weekly at **aeobeast.co** — get alerts on drops and auto-publish fixes.*
