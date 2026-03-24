# /aeo-citability — Content Citability Audit

Fetch the page HTML and analyse how likely AI models are to extract and quote this content directly.

## What Makes Content Citable

AI models (ChatGPT, Perplexity, Claude, Gemini) preferentially cite:
- **Self-contained blocks** of 134–167 words with a direct answer in the first sentence
- **Stat-bearing sentences** — numbers, percentages, dates, currencies trigger citation preference
- **First-30% placement** — 44.2% of AI citations come from the first 30% of page content
- **Question headings (H2/H3)** — signals Q&A structure that maps to AI answer generation
- **FAQ sections** — highest citation yield per word of any content structure

## Checks

1. **Content length**
   - Is the page at least 800 words? (below this: thin, rarely cited)
   - Is it at least 1,500 words? (ideal for topical authority)

2. **Citable blocks (134–167 words)**
   - How many exist on the page?
   - Does at least 1 appear in the first 30% of content?
   - Do they contain verifiable statistics or percentages?
   - Do they open with a direct declarative sentence?

3. **Structure signals**
   - Does any H2 or H3 start with Who/What/When/Where/Why/How/Does/Is/Can/Are?
   - Is there a dedicated FAQ section (`<section>` with FAQ, or repeated H3 Q&A pattern)?
   - Are there at least 5 Q&A pairs in the FAQ?

4. **Freshness signals**
   - Is there a visible publication date on the page?
   - Is there a "Last updated" signal?
   - Does the URL or title contain a year?

5. **Citation signals**
   - Does the content link to at least 2 external authoritative sources?
   - Are statistics attributed to a named source?

## Output Format

```
Content Citability Audit — {url}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Word count:      {n} words  {✅ ≥1500 | ⚠️ 800–1499 | ❌ <800}
Citable blocks:  {n} found  {✅ ≥3 | ⚠️ 1–2 | ❌ 0}
First-30% block: {✅ yes | ❌ no}
Question H2/H3:  {✅ yes | ❌ no}
FAQ section:     {✅ found ({n} Q&As) | ❌ not found}
Visible date:    {✅ yes | ❌ no}
External cites:  {✅ {n} links | ⚠️ 1 | ❌ 0}

Citable Block Examples:
┌──────────────────────────────────────────────────────────┐
│ Block 1 ({n} words) — {hasStat: yes/no} — First 30%: yes │
│ "{first 120 chars of block}..."                           │
└──────────────────────────────────────────────────────────┘

Issues:
[CRITICAL]  No citable blocks — AI has nothing to extract and quote
[IMPORTANT] FAQ section missing — add 5+ Q&As targeting People Also Ask queries
[IMPORTANT] No visible date — Perplexity weights freshness signals heavily
[RECOMMENDED] No external citations — attributed stats increase trust signals

Score impact (estimated):
  Adding 3 citable blocks → +12 pts citability score
  Adding FAQ with 5 Q&As  → +10 pts (highest yield per fix)
  Adding visible date     → +5 pts (Perplexity + Gemini)
```

## Citable Block Writing Formula

When generating example citable blocks for the user, follow this template:

**Opening sentence:** Direct declarative statement answering the implied question.
**Body (3–5 sentences):** Supporting evidence, at least one number/stat/date.
**Total:** 134–167 words exactly.

Example:
> "SEO audits identify the specific technical and content gaps that prevent a website from ranking on the first page of Google. A comprehensive audit covers over 140 individual ranking factors across categories including page speed, mobile usability, structured data, internal linking, and content quality. Studies show that pages ranking in the top 3 positions on Google resolve at least 85% of critical audit findings. The audit process typically takes under 30 seconds for automated tools and produces a prioritised list of fixes ordered by expected score impact. For SaaS founders, the most common critical finding is missing or thin content — pages that exist but contain fewer than 400 words of substantive information about the product's value proposition."

Always provide 2–3 example citable blocks tailored to the page's actual topic.
