# Revology Cars — About Us page

A rebuilt, SEO-optimized **About Us** page for [revologycars.com](https://revologycars.com/), replacing the thin existing page at `/about-us/`.

Static single-file HTML. No build step, no dependencies.

---

## Deploy

Vercel auto-detects this as a static site.

| Setting | Value |
|---|---|
| Framework Preset | **Other** |
| Build Command | *(leave empty)* |
| Output Directory | *(leave empty — repo root)* |
| Install Command | *(leave empty)* |

`index.html` is served at `/`. `vercel.json` also rewrites `/about-us` and `/about` to the same page so the staging URL matches the intended production path.

### A note on `X-Robots-Tag`

`vercel.json` sends `X-Robots-Tag: noindex, nofollow` on every response. This keeps the Vercel preview out of search results so it can't compete with the live page — the `<link rel="canonical">` in the HTML already points at the production URL `https://revologycars.com/about-us/`.

**Remove that header** if Vercel ever becomes the live host.

---

## Structure

```
index.html     the page (HTML + CSS + JSON-LD, all inline)
vercel.json    routing, security headers, noindex
```

One H1 and seven H2s, matching the agreed outline:

1. `H1` About Revology: Redefining the Classic Mustang
2. `H2` The Origin Story of Revology Cars
3. `H2` Re-Engineering Classic Mustangs With Modern Technology
4. `H2` Automotive Engineering and Manufacturing at Revology
5. `H2` Our Commitment to Quality, Performance & Authenticity
6. `H2` Automotive Expertise Behind Every Revology Mustang
7. `H2` See Where Your Revology Is Built
8. `H2` Build Your Revology

---

## Brand system

Extracted from the live site's Elementor kit (`post-37123.css`) rather than invented, so the page matches production exactly.

| Role | Value |
|---|---|
| Display | `Akhand` Bold — uppercase, line-height 1 |
| Utility labels | `Eurostile Extended Black` — uppercase, letter-spaced |
| Nav | `GT America Standard` — uppercase, .82rem, centred wordmark |
| Body | `GT America Standard` |
| Ink | `#151515` |
| Raised surface | `#212121` |
| Bone | `#F0EFE6` |
| Brand orange (accent) | `#EC5516` — from the live header nav Contact button |
| Accent hover | `#FF6A33` |
| Accent on bone | `#B8400E` — darkened for small labels |
| Muted text | `#9D9D9D` (dark) / `#6B6454` (bone) |
| Buttons | `border-radius: 100px` |

All six brand webfonts are loaded via `@font-face` from `revologycars.com`, as are every image and the factory video.

> **No assets are bundled in this repo.** Everything is hot-linked from the live WordPress media library, which is what keeps this a single file. If the client ever prunes `/wp-content/uploads/`, the referenced paths need re-checking. Every URL in the page returned HTTP 200 at time of writing.

---

## Assets used

| Section | Asset |
|---|---|
| Hero | `2026/04/revology1.jpg` — 1970 Boss 302 rolling shot |
| Origin story | `2024/05/Tom-About-2.jpeg` — Tom Scarpello + YouTube `xlStTBAeTyA` (tenth anniversary) |
| OEM approach | `2024/05/scanning.jpg` — body shell 3D scan |
| Vehicle dynamics | `2026/07/1969-revology-mustang-boss-429-363-36.webp` — car #363 |
| Manufacturing | `2026/08/manufacturing-mindset-revology.jpeg` — body-in-white in fixture + `2025/11/revology-1969-boss-mustang-429-nov2025-29.jpeg` — finished Boss 429 |
| Team | `2025/06/dr1a*.jpeg` ×7 + `2026/04/revology-all-staff-2026-1.jpg` |
| Location | `2023/08/RevologyHQ.jpg` + YouTube `R7cSaljHbiM` ("Inside the Factory") |
| CTA | `2026/07/revology-1970-boss302-home-1.jpg` |

---

## SEO

Added over the previous page: real `<title>` and meta description, canonical, Open Graph + Twitter card, descriptive `alt` on every image, and `AboutPage` + `Organization` JSON-LD carrying founder, employee count, address and social profiles.

## Sourcing

Every factual claim comes from Revology's own published copy — the FAQ, careers, how-to-order and homepage. Figures (170 employees, 360+ cars delivered, 20 countries) are stated as of **May 2026**, per the FAQ, and are labelled as such on the page and in the footer.

Two points handled deliberately rather than glossed over:

- Factory **visits and test drives are encouraged by appointment**, but general public factory tours are not offered. The location section says so plainly.
- **Safety** is framed as the passive and active improvements over the original 1960s cars — which is what the FAQ supports. The page does not claim current FMVSS compliance.
