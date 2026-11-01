# food.sankhacooray.com

Two hosts, one repo (see README.md):
- **food.sankhacooray.com**: Cloudflare Worker `food-sankhacooray`, serves `public/` (built by `build.py` from `template.html`). Deploy: `python3 build.py && CLOUDFLARE_ACCOUNT_ID=9a1da48a0f61e7cebcba39517856d87e npx wrangler deploy`.
- **kitchen.sankhacooray.com**: closed 31 Oct 2026 (GitHub Pages turned off, DNS record removed, legacy root files deleted).

## Projects
- "Local food guides" group: Rasa (rasa.sankhacooray.com, local dishes + where to eat them) and Durio. Both repos are private in `bsc2fast`.
- A card with an `og` URL needs its host in `ALLOWED_HOSTS` in `src/worker.js`, or the live share image falls back.

## Lighthouse
<!-- lighthouse-optimized: 2026-10-31 -->
- Baseline mobile: food perf 68 / a11y 100 / BP 100 / SEO 100; kitchen perf 75 / a11y 94 / BP 100 / SEO 100.
- food: non-blocking Google Fonts; brand link aria-label removed (name = visible text); hero collage + card thumbs as WebP with 400w/800w `srcset` (`build.py` uses `public/thumbs/<name>.webp` + `-400.webp` when present); live `/api/og` images start after `load` + idle, `loading=lazy`; `public/_headers` 30-day TTL for thumbs/icons/og.png.
- kitchen: non-blocking fonts + gstatic preconnect; card titles h3→h2 (same style); `--text-muted` raised (light #7a6650, dark #9c8466) and the "in the lab" card fade moved off the status chip so it keeps 4.5:1; thumbs WebP with `srcset` + width/height.
- Left: `/api/og` serves the projects' full 1200×630 PNGs, 1 h TTL (needs Cloudflare Image Transformations to resize); kitchen cache TTL is fixed by GitHub Pages; shared scripts / gtag handled centrally.
