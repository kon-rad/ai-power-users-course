## Fixing Hermes web search (Brave → ddgs)

**Symptom:** Hermes agent's `web_search` tool kept failing.

**Diagnosis:**
- `.hermes/config.yaml` had `web.backend: brave-free`.
- No `BRAVE_SEARCH_API_KEY` existed anywhere in the vault `.env`.
- Tried to get one at brave.com/search/api — Brave now requires activating a
  paid plan before it will even generate an API key. There is no free-tier
  signup path left, so `brave-free` is a dead end for a zero-cost setup.

**Fix — switch backend to `ddgs` (DuckDuckGo, free, no API key):**

```bash
cd .hermes/hermes-agent
uv pip install ddgs --python venv/bin/python3
```

Then in `.hermes/config.yaml`:

```yaml
web:
  backend: ddgs        # was: brave-free
  use_gateway: false
```

Verified with a direct smoke test:

```python
from ddgs import DDGS
list(DDGS().text("claude code anthropic", max_results=3))
```
Returned real results — confirmed working end to end.

**Caveat:** `ddgs` is scraping-based (no official API), so it's the least
robust of Hermes's web providers — the plugin's own source notes it can
hang inside native code, and DDG occasionally rate-limits scrapers. If
failures reappear, the next step up is **Tavily** (free tier, ~1,000
searches/month, needs a quick signup at tavily.com for `TAVILY_API_KEY`,
also does page extraction which `ddgs` can't).

**Other web providers Hermes ships** (`.hermes/hermes-agent/plugins/web/`),
for reference: `firecrawl`, `parallel`, `tavily`, `exa`, `searxng` (self-hosted,
free but you run the infra), `brave-free` (now paid-plan-gated), `ddgs`,
`xai`. Precedence order and config keys are documented in
`agent/web_search_registry.py`.
