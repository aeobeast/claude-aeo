# /aeo-schema — Structured Data Audit + Generator

Fetch the page HTML, extract all `<script type="application/ld+json">` blocks, and validate for AI entity recognition. Generate missing schemas.

## Checks

1. **Organization / SoftwareApplication / Product schema**
   - Exists?
   - Has `sameAs` array with 3+ entries?
   - Has `description`?
   - Has `url`?
   - `sameAs` includes LinkedIn, Twitter/X, GitHub?
   - `sameAs` includes Wikipedia (if exists)?

2. **FAQPage schema**
   - Exists?
   - Has `mainEntity` with 5+ Question items?
   - Each Question has `acceptedAnswer`?

3. **Article / BlogPosting schema** (on content pages)
   - Exists on /blog, /post, /article pages?
   - Has `author` with Person type?
   - Has `datePublished`?
   - Has `dateModified`?

4. **WebSite schema**
   - Exists?
   - Has `SearchAction` potentialAction?

5. **BreadcrumbList** (on inner pages)
   - Exists?

6. **Format**
   - Using JSON-LD? (preferred over Microdata)
   - Valid JSON? (parse check)

## Output Format

```
Structured Data Audit — {url}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Schemas found: {list types}

Organization:  ✅ found | ⚠️ sameAs has only 1 entry | ❌ missing description
FAQPage:       ❌ not found
Article:       ✅ found | ✅ author | ⚠️ missing dateModified
WebSite:       ✅ found
BreadcrumbList: ❌ not found

Issues:
[CRITICAL] No FAQPage schema — FAQs are the #1 source of direct AI citations
[IMPORTANT] sameAs incomplete — add LinkedIn, Wikipedia, Crunchbase

--- Generated JSON-LD (add to <head>) ---

{Organization schema with placeholder sameAs}
{FAQPage schema with 5 template Q&As}
```

Always generate schemas for any missing critical types (Organization, FAQPage).
Provide paste-ready `<script type="application/ld+json">` blocks.
