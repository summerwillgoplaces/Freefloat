# ⚡ FreeFloat — Live Low-Float Momentum Scanner

*by Jhoee*

A free stock scanner that finds the day's biggest spikers on the live market and shows their **float**, **relative volume**, **float rotation**, **official trading halts with live countdowns**, and a **breaking-news feed** — all in a single HTML file. No backend, no signup, no cost. Add a free Finnhub key and prices stream **tick-by-tick in real time** over WebSocket.

**🔴 Live site: [freefloat.netlify.app](https://freefloat.netlify.app/)**

![status](https://img.shields.io/badge/cost-%240.00-22c55e) ![deps](https://img.shields.io/badge/dependencies-none-3b82f6) ![file](https://img.shields.io/badge/build-single%20HTML%20file-f5b942) ![live](https://img.shields.io/badge/live-freefloat.netlify.app-16a34a) ![realtime](https://img.shields.io/badge/streaming-WebSocket%20(free%20key)-ef4444)

## Features

### Scanner
- **Live momentum scan** — the day's top % gainers, auto-refreshing (30s / 60s / 2m / off)
- **Float data** — color-coded: 🟢 low (<20M), 🟡 mid (20–100M), 🟣 high (>100M); falls back to shares outstanding (marked `SO`) when float isn't published
- **Float rotation** — today's volume ÷ float, with a progress bar
- **RVOL** — current volume vs. 3-month average
- **Sparklines** — intraday mini-chart per row with a previous-close reference line, loaded in the background
- **Detail drawer** — click any row for a full intraday chart, open/prev close/gap %/day range, and all cached headlines
- **Gap mode** (sortable gap % column), **NEW badges** for tickers that just hit the scan, **compact density mode**, **CSV export**, sortable columns, live stat strip

### Real-time streaming (optional, still free)
- Paste a free [finnhub.io](https://finnhub.io) key in ⚙ Settings and prices update **tick-by-tick via WebSocket** — no polling, no delay, green/red glow on each tick
- Subscribes to all visible tickers + your watchlist (auto-syncs each scan, capped under the free tier's 50-symbol limit)
- Auto-reconnect with exponential backoff; status pill shows **⚡ Streaming live** when connected
- Without a key, the scanner falls back to free delayed polling — nothing breaks

### Trading halts (official data)
- **⛔ HALTED badges** with **live minutes-left countdowns**, sourced from Nasdaq's official trade-halt feed (refreshed every minute, countdowns tick every 10s)
- **Active halts panel** listing all market-wide halts, your tickers first
- Halt card in the detail drawer (halted at / resumes at / countdown)
- Plain-English reason codes (LULD volatility pause, News pending, SEC suspension…)
- An **evidence-based halt guide** in the ? help modal: halt mechanics, what research says about post-halt volatility, and risk-management principles — education, not buy/sell advice
- **⚠ HALT?** heuristic flag (big move + heavy RVOL) for *candidates*, kept visually distinct from official halts

### News
- **Spiker News feed** — merged, deduplicated headlines from every scanned ticker, newest first; 🔥 breaking treatment for news under 15 minutes
- **Latest-news column** per ticker; headlines under 1 hour glow gold

### Alerts & watchlist
- 🔔 toggle: toasts + optional sound + desktop notifications when a new spiker enters the scan, a **HOT** setup triggers (RVOL threshold + fresh news), or one of your tickers gets **halted**
- ⭐ **Watchlist** — pinned stocks sort first, bypass filters, and never disappear even after dropping off the gainers list
- **Filters & presets** — price range, min RVOL, min volume, low-float-only, plus one-click presets (Classic low float / Cheap runners / Heavy volume)

### Zero-account persistence
- Your watchlist, filters, preset, and settings live in the **URL** — bookmark it or use **Copy link** in Settings to restore your exact setup on any device
- Shared links **deliberately exclude your API key**

## How it works (and stays free)

| Data | Source |
|---|---|
| Top gainers | Yahoo `v1/finance/screener` (day_gainers) |
| Quotes, volume, market cap | Yahoo `v7/finance/quote` |
| Float shares | Yahoo `v10/finance/quoteSummary` |
| News headlines | Yahoo `v1/finance/search` |
| Intraday charts | Yahoo `v8/finance/chart` |
| **Trading halts** | **Nasdaq Trader official halt RSS feed** |
| **Real-time ticks** | **Finnhub WebSocket** (free key, direct — no proxy) |

Browsers block cross-origin requests, so Yahoo/Nasdaq calls route through **three free public CORS proxies** (allorigins, corsproxy.io, codetabs) with automatic rotation on failure. Float, news, chart, and halt lookups are cached in-memory with sensible TTLs. The Finnhub WebSocket is CORS-friendly and connects directly.

## Quick start

Easiest: visit **[freefloat.netlify.app](https://freefloat.netlify.app/)** — nothing to install. For real-time prices, grab a free key at finnhub.io and paste it in ⚙ Settings.

To run locally:

```bash
# option 1: just open it
open index.html          # macOS
