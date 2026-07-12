# Unrearix — landing page

The marketing site for the Unrearix app, served at **[unrearix.com](https://unrearix.com)**.
Jekyll, built by GitHub Pages from `master`. No JavaScript, no CDN requests.

It also hosts the two URLs the app stores require:
`/privacy/` (privacy policy) and `/terms/`.

## Run it locally

```bash
bundle install
bundle exec jekyll serve --livereload   # http://127.0.0.1:4000
```

## Copy rules — read before editing

The app was rejected once under **App Store Guideline 4.2.2** and pivoted from an
artist-promo app to a music app. The store copy is written to stay on the right
side of that line, and this site must not undo it. The rules, from
`docs/store-listing.md` §4 and `docs/redesign-4.2.2.md` in the app repo:

1. **Never claim "unlimited streaming" or "stream all songs."** In-app playback
   covers the bundled tracks (currently 5); the catalogue is 48. The honest
   framing is *"browse the full discography and play in the app."*
2. **Keep external platforms in the background.** YouTube and Instagram are
   follow CTAs, nothing more.
3. **No Spotify or Apple Music links.** Those artist accounts do not exist.
4. **iPhone only.** Never show an iPad mockup — the app does not ship for iPad.
5. Body copy says `Unrearix`. Only the wordmark is `UNREARIX`. Maker credit is
   "Made by Bullets".

A build must keep passing this:

```bash
bundle exec jekyll build
grep -rniE "stream (all|unlimited)|unlimited streaming|stream anytime|spotify|apple music|ipad" \
  _site --include="*.html" | grep -vi "apple-touch"   # must print nothing
```

## Where things live

| What | Where |
|---|---|
| All copy, links, feature list, screenshot captions | `_config.yml` |
| Brand tokens (colour, type, spacing) | `_sass/tokens.scss` — mirrors the app's `docs/design-tokens.md` |
| Page sections | `_includes/{hero,features,screens,band,download}.html` |
| Icons | `_includes/icons.html` (inline SVG sprite) |
| Legal + changelog | `_pages/` |

Colours and fonts are **tokens, not settings** — they live in SCSS, not
`_config.yml`. Change them only when the app's design tokens change.

## Refreshing the screenshots

Sources are the frameless English captures in the app repo. They already contain
the status bar and Dynamic Island, so the site only supplies a CSS bezel.

```bash
SRC=../unrearix/store-screenshots/_pipeline/sources/ios_en
for f in 1_player 2_music 3_artist 4_videos 5_create 6_lounge; do
  sips -Z 1434 "$SRC/$f.png" --out "assets/screens/$f@2x.png"
  sips -Z 717  "$SRC/$f.png" --out "assets/screens/$f.png"
  cwebp -q 82 "assets/screens/$f@2x.png" -o "assets/screens/$f@2x.webp"
  cwebp -q 82 "assets/screens/$f.png"    -o "assets/screens/$f.webp"
done
```

`cwebp` comes from `brew install webp`. The Open Graph image
(`assets/og-image.jpg`) is derived from the Play feature graphic,
`store-screenshots/feature/en.png`.

## Credits

Fonts: **Anton** and **Archivo**, both SIL Open Font License — see
`assets/fonts/*-OFL.txt`.

The Jekyll scaffolding started life as
[Automatic App Landing Page](https://github.com/emilbaehr/automatic-app-landing-page)
by Emil Baehr (MIT, see `LICENSE`). The layouts, styles and content have since
been rewritten; the licence notice is retained as MIT requires.

Site content and app assets: © Bullets. All rights reserved.
