# /aeo-entity — Brand Entity & AI Prominence Audit

Analyse how strongly AI models recognise this brand as a real, trustworthy entity. AI citations correlate 3× more with brand co-occurrence in authoritative sources than with backlink count.

## Why Entity Prominence Matters

AI models build internal representations of brands from:
1. **Knowledge Graph signals** — sameAs links to Wikipedia, Wikidata, Crunchbase, LinkedIn
2. **Co-occurrence patterns** — how often the brand appears alongside trusted entities in training data
3. **Authority domain presence** — listings on G2, Product Hunt, YCombinator, npm, GitHub
4. **Social proof signals** — verified review counts, testimonials with specifics
5. **Cross-platform consistency** — same name/description across all profiles

A brand unknown to AI is never cited, even with perfect content.

## Checks

1. **Knowledge Graph coverage**
   - Organization schema has `sameAs` array?
   - `sameAs` includes LinkedIn company page?
   - `sameAs` includes Twitter/X profile?
   - `sameAs` includes GitHub org or repo?
   - `sameAs` includes Wikipedia article (if exists)?
   - `sameAs` includes Crunchbase or Wikidata?
   - `sameAs` has 5+ entries?

2. **Authority domain presence** (check via OG tags, schema, page text for links to)
   - Product Hunt listing?
   - G2 or Capterra profile?
   - YCombinator directory?
   - npm registry (if dev tool)?
   - Crunchbase profile?
   - TechCrunch / major press mention?

3. **Open Graph tags** (social sharing = training data signals)
   - `og:title` present?
   - `og:description` present (150+ chars)?
   - `og:image` present?
   - `og:url` canonical match?
   - `twitter:card` present?

4. **Brand co-occurrence signals**
   - Does the page text mention the brand name ≥ 3× ?
   - Does it appear in the title/H1?
   - Is the brand mentioned alongside well-known entities (competitors, partners, platforms)?

5. **Social proof**
   - Does the page show customer counts, user numbers, or review counts?
   - Are testimonials attributed to real named people with company?
   - Are there press/media logos?

## Authority Domains Reference

| Domain | Weight |
|--------|--------|
| linkedin.com/company/* | High — primary professional identity |
| twitter.com / x.com | High — real-time brand signal |
| github.com | High (for tech products) |
| producthunt.com | High — SaaS launch signal |
| crunchbase.com | High — funding/company legitimacy |
| wikipedia.org | Highest — direct Knowledge Graph link |
| wikidata.org | Highest — structured entity data |
| ycombinator.com | Very high — curated credibility |
| g2.com / capterra.com | High — verified review signal |
| npmjs.com | High (for dev tools) |
| techcrunch.com | Very high — editorial coverage |
| trustpilot.com | Medium — consumer review signal |

## Output Format

```
Brand Entity Audit — {url}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Entity Recognition Score: {n}/100

Knowledge Graph:
  sameAs entries:  {n}  {✅ ≥5 | ⚠️ 2–4 | ❌ 0–1}
  LinkedIn:        {✅ linked | ❌ missing}
  Twitter/X:       {✅ linked | ❌ missing}
  GitHub:          {✅ linked | ❌ missing}
  Wikipedia:       {✅ linked | ❌ not found — check if article exists}
  Crunchbase:      {✅ linked | ❌ missing}

Authority Presence:
  Product Hunt:    {✅ referenced | ❌ not found}
  G2 / Capterra:   {✅ referenced | ❌ not found}
  npm:             {✅ referenced | ❌ not found}
  Press coverage:  {✅ found | ❌ not found}

Open Graph:
  og:title:        {✅ | ❌}
  og:description:  {✅ ({n} chars) | ⚠️ too short | ❌ missing}
  og:image:        {✅ | ❌}
  twitter:card:    {✅ | ❌}

Social Proof:
  User/customer count: {✅ mentioned | ❌ not found}
  Named testimonials:  {✅ {n} found | ❌ none}

Issues:
[CRITICAL]  No sameAs entries — AI models cannot link this brand to external entity records
[CRITICAL]  No Wikipedia article — highest-weight Knowledge Graph signal missing
[IMPORTANT] No Product Hunt listing — major SaaS entity signal for AI training data
[IMPORTANT] og:description missing — social sharing = AI training data signal
[RECOMMENDED] Add Crunchbase profile — completes the professional entity cluster

Action Plan:
1. Add sameAs to Organization schema: LinkedIn, Twitter, GitHub, Crunchbase
2. Submit Product Hunt launch (or link existing profile)
3. Create Crunchbase profile (free, 15 min)
4. Add og:description (150–160 chars) and twitter:card to <head>
5. Check if Wikipedia article qualifies (≥significant coverage in press)
```

Always provide the exact sameAs array to add, with placeholder URLs the user can fill in.
