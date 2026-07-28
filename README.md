# fastn — marketing site

A rebuild of the fastn homepage as a single, self-contained `index.html`.

No build step, no package manager, no runtime dependencies. Every asset — fonts,
icons, brand logos, the world map used by the hero globe — is inlined, so the page
makes **zero external network requests** and renders identically offline.

---

## Running it

Open `index.html` in a browser. That's it.

If you'd prefer to serve it over HTTP (recommended, so `file://` quirks don't bite):

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying

Any static host works — drop `index.html` at the root.

```bash
# Vercel
vercel deploy --prod

# Netlify
netlify deploy --prod --dir .

# GitHub Pages
# Settings → Pages → Deploy from branch → root
```

---

## What's in the page

| Section | Notes |
| --- | --- |
| Navbar | 12px-radius container, icon-led dropdowns, full mobile menu |
| Hero | Left-aligned copy, right-aligned connector globe (see below) |
| Trusted by | Continuous R→L marquee, 10 partner logos rendered monochrome |
| Recognized by | Five press cards with hover states |
| Three steps | Code window, live connect list, event stream, animated metrics |
| Embed it once | Six-card feature grid |
| Connection health | Dashboard mock with sparklines and live-updating sync times |
| Prompt CTA | Typewriter placeholder cycling real prompts |
| Rigid vs opaque | Three-way comparison, fastn column elevated |
| Bottleneck | Distinct banded section, one card per audience |
| Connectors | 40-logo marquee plus a live connector counter |
| Footer + Kai | Full footer and a replica of the Kai assistant widget |

## The hero globe

A rotating Earth built from real geography, not a decorative dot shell.

- **Land data** — Natural Earth 110m land polygons, rasterised to a 360×180
  equirectangular mask, packed 1 bit per cell and embedded as base64 (~10KB).
  The sphere samples an even lat/long grid at 3.4° and keeps only points on land,
  so recognisable continents rotate past: ~1,077 dots.
- **Camera** — perspective projection (`k = CAM/(CAM − z)`), so connector tiles
  scale 0.52×–1.37× across an orbit rather than fading linearly.
- **Occlusion** — tiles passing behind the globe's silhouette are detected
  geometrically and dimmed with a depth blur. They spend ~20% of each orbit there.
- **No orbit rings are drawn.** The paths stay implied — drawing them made the
  whole thing read as an atom.

## Conventions

- **Type** — SF Pro via the Apple system stack, falling back to Segoe UI / Inter.
- **Minimum font size is 12px.** Nothing on the page goes below it.
- **Motion** — everything respects `prefers-reduced-motion`; animation loops pause
  on `visibilitychange` so background tabs cost nothing.
- **Theme** — all colour, radius and easing values are CSS custom properties in
  `:root`. Change them there, not inline.

---

## Asset provenance & licensing

Worth reading before this goes public.

- **Partner logos** (Trusted by) — supplied by fastn, converted to monochrome
  and inlined as base64 PNG. Each remains the trademark of its owner.
- **Connector icons** — paths from [Simple Icons](https://github.com/simple-icons/simple-icons)
  (CC0-1.0). Google, Slack, Gmail and Google Drive are hand-authored multicolour
  SVGs. All brand marks remain the property of their respective owners; usage
  should follow each brand's trademark guidelines.
- **World map** — [Natural Earth](https://www.naturalearthdata.com/) 110m land
  vectors, public domain, via the `world-atlas` package.
- **SF Pro** is referenced through the system font stack only. No font files are
  bundled or served, so no Apple licence is implicated.

## Known gaps

- Nav and footer links point at real paths (`/pricing`, `/docs`, …) that don't
  exist in this repo — wire them up when it's integrated.
- The Kai widget is a visual replica. Swap in the real Intercom/Kai snippet to
  make it live.
- Metrics and press quotes are carried over from the current site; re-verify
  before publishing.
