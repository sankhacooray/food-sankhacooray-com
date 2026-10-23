# food

**[food.sankhacooray.com](https://food.sankhacooray.com)** — the food, drink & taste hub in the sankhacooray.com
network: MixLab and CoffeeLab, cook and the umami pre-cook simulator, the SafeBite allergen scanner, Trophe diet
tracking, and Soundbites. Renamed from `kitchen-sankhacooray-com` (history kept).

Served by a Cloudflare Worker (`food-sankhacooray`): static assets from `public/`, plus `src/worker.js`, whose
`/api/og?url=` route fetches a project site's current `og:image` (allowlisted hosts only, cached 1 h). Cards with an
`og` page in `projects.json` show a shared fallback, then fade to that live image.
The VS Code workspace for the hub and its projects is `../food-sankhacooray-com.code-workspace`.

## Files

| Path | Purpose |
|------|---------|
| `projects.json` | Content: research areas, projects, roles, years, live links |
| `template.html` | Page template (CSS + JS inline) |
| `footer.html` | One-line footer, placed in the last section |
| `build.py` | Renders `public/index.html`; packs small areas side by side so no screen shows lonely cards |
| `public/thumbs/` | Screenshots for projects without their own share image |
| `public/og.png` | 1200×630 share image, rendered from `og/og.html` |
| `public/icon*.png`, `icon.svg`, `apple-touch-icon.png`, `manifest.webmanifest` | Icons + PWA manifest |
| `src/worker.js` | Serves assets; `/api/og` live share-image proxy |
| `wrangler.toml` | Worker config + custom domain (creates the DNS record) |

**Legacy kitchen files.** `index.html`, `CNAME`, `og-image.png`, the root icons, `manifest.webmanifest` and `thumbs/`
at the repo root are the old kitchen.sankhacooray.com site, still served by GitHub Pages. Remove them and turn off
Pages when kitchen is retired.

## Edit, preview, deploy

```
python3 build.py
python3 -m http.server 4797 --bind 127.0.0.1 -d public
CLOUDFLARE_ACCOUNT_ID=9a1da48a0f61e7cebcba39517856d87e npx wrangler deploy
```

Share image: edit `og/og.html`, render it, and bump `?v=N` on `og:image`/`twitter:image` in `template.html`:

```
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --hide-scrollbars \
  --force-device-scale-factor=1 --window-size=1200,630 --virtual-time-budget=6000 \
  --allow-file-access-from-files --screenshot=public/og.png "file://$PWD/og/og.html"
```
