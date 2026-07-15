# Leylines of Power — Marketing / Hub Site

The public-facing site for the TCG **Leylines of Power**, meant to live at the bare `leylinesofpower.com` domain and link out to the sandbox simulator at [play.leylinesofpower.com](https://play.leylinesofpower.com) (separate repo: `LoP-Simulator`). This site is content/marketing, not an app — rules, lore, news, and eventually a card gallery, none of which need the simulator's React/Vite stack.

## Status

**v1: a single landing page.** Logo, tagline, pitch copy, and a "Play Now" button to the simulator, built on a shared header/footer layout so more pages are cheap to add later. Not yet deployed — pushed to GitHub, but no Vercel project or DNS pointed at it yet (that's the natural next step once the page copy/design is signed off).

Deliberately out of scope for now: Rules/Lore/News content pages and the card gallery (+ the shared card-data package it would need) — deferred until the Awakening card set stabilizes. There's a large amount of existing brand material (lore text, rulebook PDFs, an intro video, concept art, narrated lore audio) sitting outside this repo, in the user's `Downloads/` folder, waiting for that point.

## Running it

```bash
npm install
npm run dev
```

Opens at `http://localhost:4321`.

## Tech stack

- **Astro** — chosen over the simulator's React/Vite stack specifically because this is a content site, not an interactive app: ships ~zero JS by default (fast, good for SEO), and file-based routing means adding a Rules/Lore/News page later is just a new `.astro` file in `src/pages/`, no routing library to configure. Can still mount React "islands" for anything genuinely interactive later (e.g. the eventual card gallery).
- Deploys to Vercel with zero config, same as the simulator.

## Project layout

- `src/pages/index.astro` — the landing page.
- `src/layouts/BaseLayout.astro` — shared header/footer shell (logo, nav, "Play Now" link) that future pages reuse.
- `src/styles/global.css` — brand palette (gold/dark) kept in sync **by hand** with `LoP-Simulator/src/index.css` so the two sites read as one brand. If you tweak colors in the simulator, mirror the change here.
- `public/icons/lop-logo.png` — the shared logo asset, copied from the simulator repo (not re-derived).
