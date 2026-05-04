# Lab Consumables Supply Risk Monitor

A prototype web app for monitoring open-source supply-risk signals related to petri dishes, agar plates, sterile plasticware, microbiology consumables, and upstream petrochemical feedstocks across **Thailand, Japan, Korea, China, Australia, Vietnam, Indonesia, Malaysia, Singapore, and the Philippines** — and translating those signals into potential Petrifilm conversion opportunities.

> **Notice:** This prototype uses only open / public information and does not include internal company or customer data.

---

## Features

- **Dashboard** of risk-signal cards with at-a-glance risk badges (High / Medium / Low).
- **Filters** for country, risk level, and signal type.
- **Search** across headlines, sources, explanations, and local-language headlines.
- **Hero stats** summarising active signals, high-risk counts, and country coverage.
- **Local-language coverage** — 139 search keywords across 10 APAC countries (Thai, Japanese, Korean, Chinese, Vietnamese, Indonesian, Malay, Filipino, English) browsable via the *Keyword Coverage* modal.
- **Semi-automated RSS refresh** — fetches Google News RSS for the top 3 keywords per country (max 30 feeds) via a free public CORS proxy, parses titles / links / dates / sources, classifies risk + signal type with multilingual heuristics, deduplicates, and caches in `localStorage` for 24 h.
- **Sample / RSS toggle** — switch between the curated sample dataset and live RSS results. If RSS fetch fails, the app automatically falls back to sample data with a visible warning.
- **Manual Refresh** button bypasses the 24 h cache.
- Each card includes:
  - Headline (translated where applicable), source name, published date, country
  - Original-language headline (when local-language)
  - Risk level (High / Medium / Low)
  - Signal type
  - "Why it matters" explanation
  - Potential Petrifilm conversion opportunity
  - **Open Source** button → article-level URL (always opens in a new tab)
  - **Translate Source** button → Google Translate URL, shown only for non-English originals
- Built with **vanilla HTML + CSS + JS** — no build step, no API keys, no paid services.

## Tech & constraints

- 100% client-side, single `index.html` file.
- No API keys required.
- No paid APIs used.
- No internal company or customer data.
- RSS is fetched via free public CORS proxies (`api.allorigins.win`, `corsproxy.io`); proxy availability can vary, which is why sample-data fallback exists.

## Run locally

Just open `index.html` in any modern browser. No server required.

```bash
# Or serve with any static server
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

1. Create a new public GitHub repository (e.g. `lab-risk-monitor`).
2. Commit `index.html` (and this `README.md`) to the `main` branch.
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Pick branch `main` and folder `/ (root)`. Click **Save**.
6. Wait ~1 minute. Your site is live at `https://<your-user>.github.io/<repo-name>/`.

That's it — no workflows or build step needed.

## RSS daily-refresh behaviour

This is a static site, so "daily refresh" is implemented browser-side:

- On page load, the app checks `localStorage` for a cached RSS payload.
- If the cache is **less than 24 hours old**, cached results are served when the user switches to RSS mode.
- If older or absent, the user's first switch to RSS mode (or pressing **Refresh**) triggers a fresh fetch and rewrites the cache.
- Pressing **Refresh** always forces a fresh fetch.

There is no server-side scheduler. To get genuinely scheduled refreshes, run a small GitHub Actions workflow that pre-fetches the RSS feeds nightly and commits a JSON snapshot to the repo, then have the app load that JSON instead of (or alongside) the live proxy fetch. The current schema is identical, so swapping is a small change.

## Configuration

All knobs live at the top of the JS in `index.html`:

- `LOCAL_LANGUAGE_KEYWORDS` — per-country keyword sets.
- `RSS_CONFIG` — proxies, locale hints, cache TTL, performance guardrails (top N keywords per country, max feeds per refresh).
- `RISK_HEURISTICS` and `SIGNAL_TYPE_RULES` — multilingual classifiers.
- `SIGNALS` — the curated sample dataset used as fallback.

## Signal types tracked

**Commercial opportunity signals**
- petri dish shortage
- agar plate delay
- microbiology supplies shortage
- sterile plasticware shortage
- petri dish supply disruption

**Upstream / risk signals**
- polystyrene supply
- naphtha disruption
- petrochemical disruption
- plastic consumables shortage
- laboratory consumables shortage

## License

Prototype for evaluation purposes. Public information only.
