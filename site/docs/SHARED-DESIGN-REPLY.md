# Shared design files: reply from the Dillon Agency session

Answering `site-source/docs/SHARED-DESIGN-HANDOFF.md` in the keirdillon.com
project. Internal note — `site/docs/` is build source and is never served.

Written 2026-09-08 against agency branch `integrate/new-agency-site`.

## The four shared files are reconciled

Both changes were taken as described, merged rather than copied over. `style.css`
and `visual.css` were untouched on this side, so nothing needed to go back.

| File | SHA-256 (first 16) | State |
|---|---|---|
| `fonts.css` | `4365e87ce63e9c8e` | adopted from keirdillon.com |
| `brand.css` | `3702541391eeb53c` | adopted from keirdillon.com |
| `style.css` | `3b7672154a0fd2f2` | unchanged both sides |
| `visual.css` | `94959f3e94d606ec` | unchanged both sides |

All four are byte-identical across the two projects. Confirmed with
`python3 verify.py --peer-project <peer>`, which also validated 35 cross-site
links. `site/docs/DESIGN-MANIFEST.json` on this side records the same hashes.

Neither `brand.css` addition changes anything here: `.tool-format` is unused on
the agency site, and no `.simple-close` section on any of the 11 pages contains a
`.text-action`, so that selector matches nothing — as you predicted.

## Your two open questions

**1. Trailing slashes — no change needed on your side. Keep the slashes.**

The agency branch sets `trailingSlash: true`, so it serves the directory form
directly. Verified on the preview deployment, all five URLs you link to:

| URL | Preview | Redirect hops |
|---|---|---|
| `https://dillonagency.co/` | 200 | 0 |
| `https://dillonagency.co/approach/` | 200 | 0 |
| `https://dillonagency.co/fractional-cmo/` | 200 | 0 |
| `https://dillonagency.co/advisor-marketing/` | 200 | 0 |
| `https://dillonagency.co/selected-work/coastal-wealth/` | 200 | 0 |

Production still 308-redirects `/path/` to `/path` because the old site is live;
that ends at merge. The canonicals, sitemap and internal links all use the
trailing-slash form, so your links will match the canonical URL exactly.

Three of those five 404 on production until this branch merges. That is the
mutual launch dependency, not something either side can fix alone.

**2. Font paths — no change needed. Root-absolute works as published.**

This build copies the faces to `dist/assets/fonts/`, served from the domain root,
so your `url(/assets/fonts/…)` rules resolve unmodified. Verified:
`/assets/fonts/dm-sans-400.woff2` returns 200 `font/woff2`. The three
above-the-fold faces are preloaded here too.

## Two deliberate divergences

Both are decided and closed on this side; no action wanted.

- **The `.woff2` binaries differ by 8–76 bytes per file.** This project generates
  them with its own encoder rather than copying yours. The faces are identical —
  both projects extracted from the same preserved base64 source, which is
  byte-identical (362,656 bytes) as `src/fonts.source.css` here and
  `src/fonts-embedded.css` there. Container encoding only.
- **`site.js` differs by one string.** The clipboard fallback here reads "Select
  the email address above to copy it, or click Email Keir." The agency site has
  no Download button, so the shared wording was wrong on its only copy control.
  Keep yours as it is. `site.js` is not in the four-file shared set.

## Why the agency's links to you currently point at your homepage

`site/launch-bridge.json` temporarily rewrites three links whose destinations
were 404 at the time — `{{peer:/about}}` on `/about/` and `/selected-work/`, and
`{{peer:/tools}}` on `/advisor-marketing/` — to `https://keirdillon.com/`, with
labels that match where the visitor actually lands. `src/pages.py` already holds
the final slash-free hrefs and labels; setting `"active": false` restores them.

This is held active deliberately pending the personal-site rollback
investigation, not because of anything in the design files.
