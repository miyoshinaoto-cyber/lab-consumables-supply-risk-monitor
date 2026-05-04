# Lab Plastic Supply Chain Risk Monitor

A browser-based early-warning dashboard for **lab-plastic supply-chain risk** — covering Tier 1 lab consumables (petri dishes, agar plates, sterile plasticware), Tier 2 adjacent labware (lab consumables, single-use lab plastics), and Tier 3 upstream petrochemical inputs (naphtha, polystyrene, polypropylene, resin, polymer, plastic feedstock). Monitors **Thailand, Japan, Korea, China, and Australia** with native local-language search.

> **Notice:** This tool uses only open / public information. No internal company or customer data, no API keys, no paid services. Open-source signals only.

---

## What this is

This is **not a news reader**. It's a supply-chain early-warning surface designed around three principles:

1. **Three-tier monitoring scope.** A petri-dish shortage matters; so does a polystyrene plant shutdown six months upstream. The classifier scores both.
2. **Real RSS only.** No synthetic sample data. The Sample dataset is bootstrapped from the user's first real RSS refresh and persisted in `localStorage`. A "Rebuild Sample from RSS" button refreshes it on demand.
3. **Local-language native.** Each country uses its own Google News locale (`hl=ja&gl=JP`, `hl=th&gl=TH`, `hl=ko&gl=KR`, `hl=zh-CN&gl=CN`, `hl=en-AU&gl=AU`) so headlines come back in the language they were published in.

## Signal classification

Every fetched headline runs through a single shared `classifySignal(articleText)` function:

| Tier | Result type     | Trigger                                          | Score              |
|------|-----------------|--------------------------------------------------|--------------------|
| 1    | Direct Signal   | Tier-1 term + supply term                        | High / Medium      |
| 2    | Adjacent Signal | Tier-2 term + supply term                        | Medium             |
| 3    | Upstream Signal | Tier-3 term + supply term                        | Medium             |
| –    | Weak Signal     | Any tier term alone, no supply term yet          | Low                |
| –    | Filtered        | Hits the exclusion list, or no tier term matches | Filtered (hidden)  |

**Supply terms** include: shortage · delay · lead time · supply disruption · supply issue · supply constraint · backorder · unavailable · plant shutdown · production cut · capacity reduction · price surge / pressure · tight supply · restructuring · uncertainty · volatility — plus the same in Japanese, Thai, Korean, Chinese.

**Strict exclusions** (phrase-level so they don't kill legitimate supply news): research papers · arXiv preprints · AI / machine-learning / simulation · organoids / brain cells / lab-grown meat · sports / abuse / celebrity · plastic-waste / recycling / environmental commentary · pure outbreak framing without supply context.

## Zero-result fallback

If a fresh RSS refresh produces zero items in Direct / Adjacent / Upstream — but some fetched items mention any tier term — the top 3 highest-tier-density items are promoted to Weak Signals so RSS mode never returns an empty page.

## Sample data — RSS-bootstrapped

There is **no synthetic sample data** in this build. The first time you click **Refresh** (or **Rebuild Sample from RSS**), the top 5–8 highest-scoring relevant items from that fetch are persisted to `localStorage` (`lrm_sample_v1`) and become the Sample dataset. A label *"Sample Data is based on real RSS results · built [time] · N items"* makes the source explicit. Switching to Sample mode before any refresh shows an empty-sample prompt explaining how to populate.

## Open Source links

Each card has an **Open Source** button. URL resolution policy:

1. Real article URL extracted from the RSS `<description>` (preferred — bypasses the Google News redirect)
2. Google News article link (`news.google.com/rss/articles/...`) — accepted because it redirects to the article
3. Otherwise the button is **disabled** with the label *"Source URL unavailable"*. Bare publisher homepages are never used as fallback.

## Debug surface

A *"Show filtered RSS items"* toggle appears in RSS mode. Enabling it reveals a section beneath the main grid listing every filtered item with: headline, source / date / country, the originating Google News query keyword, the filter reason in amber, and term-chip rows showing exactly which tier-1 / tier-2 / tier-3 / supply / exclusion terms matched (or didn't). A "Relevance reason" debug row appears on every card.

## Stats

Both modes show: `X direct · Y adjacent · Z upstream · W weak · F filtered`.

## Tech & constraints

- 100% client-side, single `index.html`, no build step.
- Vanilla HTML / CSS / JS only.
- Free public CORS proxies (`api.allorigins.win`, `corsproxy.io`) — rotated with timeout fallback.
- `localStorage` for the 24-hour RSS cache (`lrm_rss_cache_v3`) and the bootstrapped sample (`lrm_sample_v1`).

## Run locally

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

1. Push `index.html` and `README.md` to a public repo's `main` branch.
2. Repo **Settings → Pages**, source: **Deploy from a branch**, branch: `main`, folder: `/ (root)`.
3. Live at `https://<your-user>.github.io/<repo-name>/` within ~1 minute.

## Configuration

All knobs sit at the top of the JS in `index.html`:

| Constant | Purpose |
|---|---|
| `RELEVANCE_FILTERS.tier1Direct` / `tier2Adjacent` / `tier3Upstream` | 3-tier term lists |
| `RELEVANCE_FILTERS.supply` | Supply-pressure terms (used across all tiers) |
| `RELEVANCE_FILTERS.exclude` | Phrase-level exclusion list |
| `LOCAL_LANGUAGE_KEYWORDS` | Per-country search keywords (2 per country, native script) |
| `RSS_CONFIG.localeHints` | Per-country `hl` / `gl` / `ceid` |
| `RSS_CONFIG.maxFeedsPerRefresh` | 10 |
| `DEBUG_FLAGS.fallbackTopWeak` | How many to promote to Weak when relevant pool is empty (default 3) |
| `DEBUG_FLAGS.softExclusion` | Whether exclusion hits surface a clear reason in the debug UI |
| `SAMPLE_BOOTSTRAP_TARGET` | How many items the Sample dataset captures on rebuild (default 8) |

## License

Prototype for evaluation purposes. Public information only.
