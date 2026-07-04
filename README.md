# ⚡ FreeFloat — Live Low-Float Momentum Scanner

A free, zero-API-key stock scanner that finds the day's biggest spikers on the live market and shows their **float**, **relative volume**, **float rotation**, and **latest news headline** — all in a single HTML file. No backend, no signup, no cost.

**🔴 Live site: [freefloat.netlify.app](https://freefloat.netlify.app/)**

![status](https://img.shields.io/badge/cost-%240.00-22c55e) ![deps](https://img.shields.io/badge/dependencies-none-3b82f6) ![file](https://img.shields.io/badge/build-single%20HTML%20file-f5b942) ![live](https://img.shields.io/badge/live-freefloat.netlify.app-16a34a)

## Features

- **Live momentum scan** — pulls the day's top % gainers and auto-refreshes (30s / 60s / 2m / off)
- **Float data** — free-float shares per ticker, color-coded: 🟢 low (<20M), 🟡 mid (20–100M), 🟣 high (>100M). Falls back to shares outstanding (marked `SO`) when float isn't published
- **Float rotation** — today's volume ÷ float, with a progress bar showing how many times the float has traded
- **RVOL** — current volume vs. 3-month average
- **Latest news column** — most recent headline per ticker with age stamp; headlines under 1 hour glow gold (catalyst spotting)
- **Filters** — price range, min RVOL, min volume, and a one-click "low float only" toggle; applied instantly, no refetch
- **Sortable columns**, live stat strip, rows flash green when a stock's gain ticks up between scans

## How it works (and stays free)

Data comes from Yahoo Finance's free public endpoints:

| Data | Endpoint |
|---|---|
| Top gainers | `v1/finance/screener/predefined/saved?scrIds=day_gainers` |
| Quotes, volume, market cap | `v7/finance/quote` |
| Float shares | `v10/finance/quoteSummary` (`defaultKeyStatistics`) |
| News headlines | `v1/finance/search` |

Browsers block cross-origin requests, so the app routes calls through **three free public CORS proxies** (allorigins, corsproxy.io, codetabs) and automatically rotates to the next one if a proxy rate-limits or fails. Float and news lookups are cached in-memory (news: 5-minute TTL) to keep refresh cycles light.

## Quick start

Easiest: just visit **[freefloat.netlify.app](https://freefloat.netlify.app/)** — nothing to install.

To run it locally instead:

```bash
# option 1: just open it
open index.html          # macOS
start index.html         # Windows

# option 2: serve locally
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deployment

The site is live at **[freefloat.netlify.app](https://freefloat.netlify.app/)**, hosted free on Netlify.

To update it: log in to Netlify → *Deploys* → drag the new `index.html` onto the deploy area. Or connect this GitHub repo under *Site configuration → Build & deploy*, and every push to `main` auto-deploys.

Other free hosts if you want a mirror:

- **GitHub Pages** — *Settings → Pages → Deploy from branch (main)* on this repo → `yourusername.github.io/freefloat`
- **Neocities** — upload at [neocities.org](https://neocities.org) → `freefloat.neocities.org`

## Known limitations

- Free public proxies can rate-limit during heavy traffic (e.g., market open). The app rotates proxies and retries on the next cycle automatically — worst case, hit **Scan now** after ~30 seconds
- Yahoo's endpoints are unofficial and undocumented; data is delayed and fields can occasionally be missing
- First scan takes a few seconds longer while float + news lookups populate the cache

## Disclaimer

This tool is for **informational and educational purposes only**. It is not financial advice, and the data (delayed, unofficial, third-party) should not be relied on for trading decisions. Low-float stocks are extremely volatile — do your own research.

## License

MIT — do whatever you want with it.
