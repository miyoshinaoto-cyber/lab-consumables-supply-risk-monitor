# Lab Consumables Supply Risk Monitor

A prototype web app for monitoring open-source supply-risk signals related to petri dishes, agar plates, sterile plasticware, microbiology consumables, and upstream petrochemical feedstocks across **Thailand, Japan, Korea, China, Australia, Vietnam, Indonesia, Malaysia, Singapore, and the Philippines** — and translating those signals into potential Petrifilm conversion opportunities.

> **Notice:** This prototype uses only open / public information and does not include internal company or customer data.

---

## Features

- **Dashboard** of risk-signal cards with at-a-glance risk badges (High / Medium / Low).
- **Filters** for country, risk level, and signal type.
- **Search** across headlines, sources, and explanations.
- **Hero stats** summarising active signals, high-risk counts, and country coverage.
- Each card includes:
  - Headline, source name, published date, country
  - Risk level (High / Medium / Low)
  - Signal type
  - "Why it matters" explanation
  - Potential Petrifilm conversion opportunity
  - **Open Source** button linking to the original public URL
- Built with **vanilla HTML + CSS + JS** — no build step, no API keys, no paid services.

## Tech & constraints

- 100% client-side, single `index.html` file.
- No API keys required.
- No paid APIs used.
- No internal company or customer data.
- Sample data is illustrative; structure is ready to swap in live RSS feeds via a public CORS proxy or static JSON drops.

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

## Extending with live data (optional)

The sample-data array `SIGNALS` in `index.html` is the only place data lives. To plug in live open-source feeds later:

- Replace the static array with a `fetch()` to a JSON file you generate from RSS feeds.
- Or use a free public RSS-to-JSON proxy and merge multiple feeds client-side.
- Keep the field shape: `{ id, headline, source, url, date, country, risk, signal, why, opportunity }`.

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
