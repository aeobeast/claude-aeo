# /aeo-llmstxt — llms.txt Audit + Generator

Fetch `{domain}/llms.txt` and validate compliance with the llmstxt.org specification. If missing or incomplete, generate a production-ready replacement.

## The llms.txt Spec (llmstxt.org)

llms.txt is a markdown file at the site root that tells AI models what content exists and how to navigate it. AI agents (coding assistants, research tools, agentic crawlers) prioritise this file when building context about a site.

**Required structure:**
```
# {H1 — Brand/Product Name}

> {blockquote summary — 1-3 sentences describing what the site is}

## {Section Name}

- [{Link Title}]({url}): {description of what's at this URL}
```

**Rules:**
1. Must start with H1 (the brand/product name)
2. Must have a blockquote (`>`) immediately after H1 as the summary
3. Must have at least one H2 section grouping links
4. Links must include colon + description after the URL
5. Optional but recommended: `## Optional` section for supplementary links

## Checks

1. **Existence**
   - Does `{domain}/llms.txt` return 200?
   - Does `{domain}/llms-full.txt` exist? (extended version)

2. **Structure compliance**
   - Starts with H1?
   - H1 is the product/brand name (not generic)?
   - Has blockquote summary immediately after H1?
   - Blockquote is ≥ 20 words?
   - Has at least 1 H2 section?
   - Has at least 3 linked URLs?

3. **Link quality**
   - Each link has a description after `:` ?
   - Links go to real, useful pages (not 404)?
   - Covers key pages: docs, blog, about, pricing, API reference?

4. **Content breadth**
   - Does it include the homepage?
   - Does it include documentation or help content?
   - Does it include blog/articles?
   - Does it include a sitemap or content index page?

5. **robots.txt alignment**
   - Is `/llms.txt` allowed for citation bots?
   - Does `robots.txt` reference or allow AI crawlers access to the path?

## Output Format

```
llms.txt Audit — {url}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Status:    {✅ found at /llms.txt | ❌ not found}
H1:        {✅ "{title}" | ❌ missing}
Summary:   {✅ blockquote found ({n} words) | ❌ missing}
Sections:  {✅ {n} H2 sections | ⚠️ 1 section | ❌ none}
Links:     {✅ {n} links with descriptions | ⚠️ links missing descriptions | ❌ no links}

Issues:
[CRITICAL]  No llms.txt — AI agents cannot efficiently navigate this site's content
[IMPORTANT] No blockquote summary — AI models use this as the site's canonical description
[RECOMMENDED] Add docs/help section — highest-value content for agentic AI usage

--- Generated llms.txt (publish at {domain}/llms.txt) ---

# {Brand Name}

> {AI-extracted 2-sentence description of what the product does and who it's for, extracted from the page's title, meta description, and hero copy}

## Pages

- [{Page Title}]({url}): {1-sentence description extracted from page meta description or H1}
[one entry per discovered page — homepage, pricing, about, blog index]

## Documentation
[only if docs/ or help/ links found on page]

- [Getting Started]({url}): Step-by-step guide to setting up {product}
[etc]

## Optional

- [Sitemap]({domain}/sitemap.xml): Full index of all pages
```

## Generation Rules

When generating llms.txt for a site:
1. Extract brand name from `og:site_name` or `<title>` tag
2. Write blockquote from meta description + first H1/H2 of homepage
3. Discover links from: nav links, footer links, sitemap (if accessible), `<a>` tags with keywords (docs, blog, about, pricing, api, help, guides)
4. Write link descriptions from: `<title>` of linked page, or infer from URL + anchor text
5. Group into logical H2 sections: Pages, Documentation, Blog, API Reference
6. Add `## Optional` with sitemap.xml link

Always generate a complete, paste-ready file. Never leave placeholder text like `{Brand Name}` — extract real values from the page.
