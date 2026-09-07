# dillonagency.co

Dillon Agency is the commercial business: fractional CMO leadership, brand and
recruiting strategy, advisor marketing, marketing systems, and implementation
for wealth management firms. Led by Keir Dillon.

The personal platform — Keir's perspective, full career story, writing, advisor
tools and community — lives separately at **keirdillon.com**. Both sites carry
the same Rams design system and link to each other. See
`site/docs/BRAND-ARCHITECTURE.md` for the content boundary between them.

## Layout

```
site/
  build.py            Dependency-free static builder (Python 3.9+, no pip/npm)
  build_vercel.py     Environment-aware adapter used by the Vercel build
  verify.py           Structural, asset, link, and metadata checks
  site.json           Domain, peer domain, navigation, contact address
  src/
    pages.py          Every page's copy and metadata — the content authority
    components.py     Photo and contact helpers
    fonts.css         Generated: @font-face rules pointing at fonts/*.woff2
    fonts.source.css  Original supplied faces, base64 (kept as the master)
    style.css         Shared Rams design system   ┐ identical in both
    visual.css        Editorial layout + imagery  │ brand projects; change
    brand.css         Brand-architecture edition  ┘ them in both or neither
    site.js           Menu, standalone navigation, copy-to-clipboard
    assets/           Image masters — never modified, hash-checked by verify.py
    assets.json       Master dimensions, provenance, SHA-256
    derivatives/      Generated: responsive AVIF/WebP/fallback ladders
    derivatives.json  Generated: srcset ladders and per-image `sizes`
    fonts/            Generated: WOFF2 faces
    static/           Copied verbatim into the output (icons, social card,
                      and the brand marks retained at /assets/images/)
  tools/              Local-only asset pipeline (Node + sharp). Not run on Vercel.
  docs/              Internal architecture, route map, handoff. Never served.
vercel.json           Build config, route redirects, headers, caching
```

Only `site/dist/` is served. The source, masters, docs and handoff material stay
out of the public output by construction.

## Build

```sh
cd site
python3 build.py                                    # review build -> dist/ (noindex)
python3 build.py --production https://dillonagency.co   # launch build -> dist/ (indexable)
python3 verify.py                                   # structure, assets, links, metadata
```

The review build also emits `dillon-agency-website.html`, a single self-contained
file with fonts and images embedded for offline review. Pass `--no-standalone`
to skip it; the Vercel build always does.

`build_vercel.py` picks the mode from `VERCEL_ENV`: production builds use the
confirmed domain and are indexable, every other environment stays `noindex,
nofollow` with a disallow-all `robots.txt`. Nothing needs to change at launch.

Preview a build locally with `python3 -m http.server -d dist 8080`.

## Regenerating assets

Fonts, responsive image derivatives, icons and the social card are generated
once and committed, so the deploy build needs no dependencies. Regenerate them
only after changing a master or a layout width:

```sh
cd site/tools && npm install && npm run prepare-assets
```

Masters in `src/assets/` are never modified — `verify.py` fails if their hashes
change. Measured results are recorded in `site/asset-report.json`.

## Contact

`keir@dillonagency.co` on both sites. The contact page opens the visitor's mail
client and offers a copy-address fallback and LinkedIn; there is no form backend,
newsletter, analytics, or payment service in this repository.

## Deployment

GitHub `keirdillon/dillonagency-site` → Vercel project `dillonagency-site` →
`dillonagency.co` (apex is canonical; `www` redirects to it). Pushing a branch
creates a preview; merging to `master` releases production.
