# /aeo-crawlers — AI Bot Access Audit

Fetch `{origin}/robots.txt` and check access for all known AI crawlers.

## Bot Registry

### Citation bots — MUST be allowed (they drive AI visibility)
| User-Agent | Platform |
|---|---|
| OAI-SearchBot | ChatGPT web search |
| ChatGPT-User | ChatGPT user-triggered |
| ClaudeBot | Claude real-time citations |
| claude-web | Claude web crawler |
| PerplexityBot | Perplexity index |
| Perplexity-User | Perplexity user-triggered |
| DuckAssistBot | DuckDuckGo AI answers |
| YouBot | You.com AI |
| MistralAI-User | Le Chat citations |
| Applebot | Apple Intelligence |
| Amazonbot | Alexa AI |
| Diffbot | Structured data extraction |

### Training bots — operator choice (blocking does NOT hurt AEO)
| User-Agent | Platform |
|---|---|
| GPTBot | OpenAI training |
| anthropic-ai | Claude training |
| Google-Extended | Gemini training |
| cohere-ai | Cohere training |
| AI2Bot | Allen Institute |
| CCBot | Common Crawl |

### Hybrid bots (search + training)
| User-Agent | Platform |
|---|---|
| Googlebot | Google Search + AI Overviews |
| Bingbot | Bing + Copilot |

## Checks

For each citation bot:
1. Is it explicitly Disallowed?
2. Is it caught by a wildcard `Disallow: /` with no override?

For robots.txt overall:
3. Does it exist?
4. Is there a `Crawl-delay` > 10s?
5. Are citation bots explicitly listed (explicit Allow)?
6. Is `Sitemap:` directive present?

## Output Format

```
AI Crawler Access Audit — {url}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

robots.txt: {found/not found}

Citation Bots Status:
✅ OAI-SearchBot — allowed
✅ ClaudeBot — allowed
❌ PerplexityBot — BLOCKED (critical)
⚠️  YouBot — not explicitly listed
...

Training Bots Status:
🔵 GPTBot — blocked (your choice)
🔵 anthropic-ai — allowed
...

Issues Found:
[CRITICAL] PerplexityBot is blocked — add: User-agent: PerplexityBot / Allow: /

Recommended robots.txt additions:
{paste-ready robots.txt snippet}
```
