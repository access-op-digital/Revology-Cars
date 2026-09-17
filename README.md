# Revology Cars — rebuilt company pages

Rebuilt, SEO-optimized pages for [revologycars.com](https://revologycars.com/), replacing thin
existing pages and adding two that don't exist yet.

Static HTML. No build step, no dependencies, no bundled assets.

| Page | File | Route on this deployment | Intended production path |
|---|---|---|---|
| Company hub | `index.html` | `/`, `/company` | `/company/` *(new — path to confirm)* |
| About Us | `about-us.html` | `/about-us`, `/about` | `/about-us/` |
| Tom Scarpello (author) | `tom-scarpello.html` | `/author/tom-scarpello`, `/meet-the-founder` | `/author/tom-scarpello/` *(new)* |
| Testimonials | `testimonials.html` | `/testimonials` | `/testimonials/` |

**The hub is the deployment root**, so <https://revology-cars.vercel.app> opens the index of the
three pages and the client can click through from there. That is a staging arrangement: on
production the site root is Revology's real homepage, which is why the hub still canonicalises
to `/company/` rather than to `/`.

All four share one header, footer, nav panel and brand CSS block, and cross-link to each other
with root-relative paths (`/`, `/about-us`, `/author/tom-scarpello`, `/testimonials`) so the
set is clickable on the preview and on production. Links to pages that already exist on
WordPress — inventory, registry, how to order, management team, careers, FAQ, contact — stay
absolute.

---

## Deploy

Vercel auto-detects this as a static site.

| Setting | Value |
|---|---|
| Framework Preset | **Other** |
| Build Command | *(leave empty)* |
| Output Directory | *(leave empty — repo root)* |
| Install Command | *(leave empty)* |

`vercel.json` sets `cleanUrls` and rewrites `/company` (to the hub at the root), `/about`,
`/author/tom-scarpello`, `/author` and `/meet-the-founder`. `/about-us` and `/testimonials`
need no rewrite — `cleanUrls` serves them from their own files. `Cache-Control:
max-age=0, must-revalidate` goes on every `.html`.

### A note on `X-Robots-Tag`

`vercel.json` sends `X-Robots-Tag: noindex, nofollow` on every response. This keeps the Vercel
preview out of search results so it can't compete with the live pages — each page's
`<link rel="canonical">` already points at its intended production URL.

**Remove that header** if Vercel ever becomes the live host.

---

## The pages

### Company hub — `index.html`

An index to the other three. One H1, three H2s: *Three Places to Start* (a row per page, with
a list of what is actually on it), *Revology Cars In Brief*, *Everywhere Else on
revologycars.com*. Carries the same six figures as the About page, quoted from it rather than
recalculated, so the two can't drift.

`CollectionPage` + `ItemList` JSON-LD naming the three pages, plus `Organization` and
`BreadcrumbList`.

### Testimonials — `testimonials.html`

Replaces a five-page paginated Elementor loop whose `<title>` was *"Classic Ford Mustang"* and
which had **no H1 at all**.

- **All 42 owner accounts on one page, no pagination.** Complete text, not the archive's
  excerpt — several accounts run to 200–380 words and were previously cut mid-sentence.
- **One account per full-width row, each labelled across the top.** Every row opens with a
  header spanning the whole width — owner, model, production number, location — above both the
  photographs and the letter. Photographs sit one side, the letter the other, sides alternating
  down the page, both columns starting at the top of the row.
- **Long reviews are clipped to the height of the photographs beside them**, with a small
  chevron — *Read the full review* — to open the rest. The accounts run from 24 to 381 words, and
  the long ones used to dwarf their own car. 27 of the 40 carry a chevron (over 70 words). The
  clip and its button are added by script, not baked into the markup, so if the script never runs
  the whole review is simply there — clipping text with no way to unclip it would be worse than
  an uneven row.
- **Every CTA sits on the same line as the foot of its photographs.** Short reviews get there
  with CSS alone: the copy column stretches to the row height and `margin-top:auto` pushes the
  button down. Long ones are pinned — script measures the photo strip and sets the copy column to
  exactly that height, and because the column is a flex stack the heading and chevron take their
  natural height, the CTA's auto margin holds the bottom, and the clipped review absorbs
  whatever is left. Nothing has to predict how many lines a heading wrapped to. It re-measures
  after webfonts land, on resize, and when a review is opened or closed; when the columns stack
  on narrow screens there is nothing to align to, so it falls back to a CSS height.

  The clip threshold is deliberately low (70 words) for one reason: a review **without** a
  chevron has no way to be opened, so it must never be able to outgrow its photographs — if it
  did, its CTA would drop below the image and break the line the others sit on. Giving a
  borderline review a chevron it barely needs is the cheaper mistake.
- **Every account names the car and links to it.** Each entry carries the model
  ("1969 Mustang Boss 429"), the production number, and a **View the build** button to that car's
  own page — so a testimonial is also a way into the product.
- **Up to four photographs per account, in a swipeable strip.** The photograph Revology
  published with the account leads, because it is the owner's own; the car's three registry
  photographs follow. Scroll-snap does the scrolling, so it swipes on touch and arrow-keys in a
  browser with no script at all — the arrows and the `1 / 4` counter are progressive
  enhancement. 28 accounts get four, six get three, six get the single photograph they came with.
- **Films sit between the accounts, not all at the top.** Three runs of cards (14 / 14 / 12),
  each followed by a full-width dark film band.
- **Films load on click.** The band shows the YouTube poster and a *Play film* button; the
  iframe is created on click, so the page costs nothing in YouTube payload until asked.
- **Tight vertical rhythm.** Forty rows compound whatever gap they are given, so the row block
  is deliberately small and the rule between rows does the separating rather than whitespace.
  The three runs of accounts also carry their own band padding (`.accounts`) instead of the
  shared 7rem, which is right for a section of prose and far too much for a list.
- Closes on a full-bleed promo band — *Your Place in Revology History* — with one primary action.

The hero runs straight into the accounts: no buttons under the lede, no figures band, and no
section heading over the run. That leaves each owner's name as the page's `h2` and their quote
heading as the `h3` beneath it, so the outline is `h1 → h2 → h3` with nothing skipped. The
counts that fed the removed figures band are still computed at build time — they keep the meta
description honest and report what the build actually found (42 accounts, cars #2–#371, 18 US
states plus Botswana and South America, 41 cars openable in the registry, 5 repeat owners).

Structure and CTA pattern are modelled on
[velocityrestorations.com/testimonials](https://www.velocityrestorations.com/testimonials/),
which alternates a full-bleed video testimonial with rows of quote cards, names the vehicle on
every testimonial and makes each one a route into that specific build. One deliberate
departure: Velocity wraps the whole card in the product link, which here would put a 380-word
letter inside a single anchor and make the text unselectable, so the action is its own button.
Their cards also carry one-line quotes where these carry the complete account, which is why
each one needs a full row rather than a column of a masonry.

#### Photographs come from each car's own registry page

`collect.js` reads every image reference off the car's registry page — most are served from the
site's CDN, so CDN paths are rewritten back to canonical `revologycars.com` URLs — drops
WordPress resizes and site furniture (logos, badges, icons), and keeps document order, which is
the order the gallery runs in. All 246 candidates were then range-requested in parallel and only
answering URLs kept: **245 of 246 live, three photographs for all 41 verified cars.** The one
failure was a mangled filename on the source page.

Because registry pages are per-car, the photographs on one belong to that car. 30 of the 41 also
carry the production number in the filename; the other eleven are 2017–2020 cars named by model
and colour instead, and the model matches in each case.

#### Films: Revology's own channel only

| Band | Video | Title | Channel |
|---|---|---|---|
| 01 | `vkkbqsv41WY` | What Does a Revology Mustang Feel Like to Drive? \| Real Driver Reactions | Revology Cars |
| 02 | `B3B2XubP_Ms` | Revology 1968 Mustang GT tackles Porsches and Ferraris on South African 1000-mile car rally | Revology Cars |
| 03 | `oJsUaMr24Ac` | Revology Testimonial — Production Car #2, #3, #17, #21 | Revology Cars |

Band 01 is a page-level film — it isn't tied to one account, so it carries its own heading and
links on to the inventory and to the
[About Revology Cars playlist](https://www.youtube.com/playlist?list=PLxaSyGmZCwoHVenIQKT6PLSVJeytH3JKo).
Bands 02 and 03 belong to the two owners whose accounts Revology filmed, and sit beside those
owners' full text and registry links.

Every title, channel and available thumbnail size was read from YouTube's oEmbed endpoint, so
they are the publisher's own wording, and `author_name` was checked to be **Revology Cars** on
each one.

> **Removed deliberately:** the live testimonial card for Kevin H. (car #61) embeds HYPEBEAST's
> *Drivers | Kevin Hart*. That is a third-party film on someone else's channel and is not
> reproduced here — his account runs as an ordinary entry instead. The builder enforces this: any
> video id not in the verified Revology list is dropped and that account falls back to a card.
> His is the one entry with no photograph of its own, so it renders as text (`.tcard--noimg`).

#### The registry links are individually verified

Car pages live at `/registry/registry-{n}/`, and **that URL cannot be trusted from the number
alone.** WordPress near-matches short slugs: `/registry/registry-2/` serves car **#20**,
`registry-3` serves **#2**, `registry-8` serves **#80**, `registry-9` serves **#90**, and
`registry-29` serves **#290**. All of them return HTTP 200, so a status check proves nothing.

`verify-registry.js` fetched all 50 numbers quoted on the page, read the "Production No." off
each rendered page, and kept the link only where it matched. **41 of 50 verified**; the other
nine (#2, #3, #8, #9, #29 mismatched, and #40, #70, #93, #94 have no page) carry no link — the
account still appears in full, just without a way through. Model names on the cards come from
the same verified pages, so they are Revology's own wording, not inferred from the quote.

### Tom Scarpello — `tom-scarpello.html`

A founder/author entity page. There is no live equivalent; the About page had a *Meet the
Founder* button deliberately left inert with a `btn--pending` class and a comment saying how to
activate it. That button is now a link to this page, and the placeholder CSS is gone.

One H1 and six H2s: *Who Tom Scarpello Is*, *The Career Behind the Cars*, *What He Knows*,
*In His Own Words*, *Interviews and Appearances*, *What He Built*.

`ProfilePage` + `Person` JSON-LD with `jobTitle`, `worksFor`, `knowsAbout`, `sameAs`
(LinkedIn) and `subjectOf` (the seven appearances). The About page's `Organization.founder` now
carries the same `@id`, so the founder is one entity across the site rather than a bare name on
each page.

---

## Brand system

Extracted from the live site's Elementor kit (`post-37123.css`) rather than invented, so the
pages match production exactly.

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

> **The brand block is duplicated in each file on purpose.** Every page is self-contained so it
> can be pasted into WordPress or Elementor without a second request for a stylesheet. The
> tokens, primitives, header, footer and figures CSS are byte-identical across the four files —
> when a token changes, change it in all four (`--accent` appears once per file, in `:root`).

> **No assets are bundled in this repo.** Everything is hot-linked from the live WordPress media
> library, which is what keeps these single files. If the client ever prunes
> `/wp-content/uploads/`, the referenced paths need re-checking. Every URL across all four pages
> returned HTTP 200 at time of writing.

---

## SEO

Added over the previous pages: real `<title>` and meta description on each, canonical to the
intended production URL, Open Graph + Twitter card, descriptive `alt` on every image, a single
H1 per page, and a linked JSON-LD graph — `AboutPage`, `ProfilePage`, `CollectionPage`,
`Person`, `Organization`, `ItemList` and `BreadcrumbList` sharing `@id`s across the four files.

### On the testimonial `Review` markup

The testimonials page carries 42 `Review` items inside an `ItemList`, each with the owner's full
`reviewBody`, name and location, plus an `about` array of `Car` entities for the verified
production cars the account is about, and a `VideoObject` on the two accounts Revology filmed.
The page-level driver-reactions film is its own `VideoObject` on the `CollectionPage`.

**There is no `reviewRating` and no `aggregateRating`, deliberately.** The owners did not give
scores, so there are none to mark up — inventing them would be fabricating data. Note also that
Google does not show review rich results for reviews a business hosts about itself, so this
markup is here for semantic clarity and citation, not for stars in the SERP.

The JSON-LD on that page is minified; pretty-printing 42 full review bodies added ~12KB of
indentation to no one's benefit.

---

## Sourcing

Every factual claim comes from Revology's own published copy — the FAQ, careers, how-to-order,
meet-the-team and homepage — or from a source that was fetched and checked while building.

- **The 42 testimonials** were parsed out of the live Elementor markup on
  `/testimonials/` pages 1–5, not retyped: quote text, headline, name, location, production-car
  number, photo URL and video ID all come straight from the source HTML. Owners' own typos and
  phrasing are preserved verbatim (one owner writes "Fasted in my group"); only photo-credit
  lines were lifted out of the quote bodies into their own caption.
- **Car models and registry links** were read off each car's own registry page and matched
  against its production number before being used — see the section above. Nine cars failed
  that check and are shown without a link rather than pointed at the wrong car.
- **Tom's career facts** — entire career in the industry; manufacturing, marketing, sales and
  product planning for Ford and Nissan in the U.S. and overseas; 17 years at Ford; ran the
  Special Vehicle Team 1998–2004 — are from
  [revologycars.com/meet-the-team/](https://revologycars.com/meet-the-team/). The Jaguar /
  Nissan / Infiniti line and both quotes come from the About page copy already signed off.
- **The seven appearances** each returned HTTP 200, and every YouTube title and channel name was
  read from YouTube's oEmbed endpoint. Only the Detroit News piece carries a date, because it is
  the only one whose date could be verified; the rest are listed without one rather than guessed.
- Figures (170 employees, 20 countries) are stated as of **May 2026**, per the FAQ, and are
  labelled as such in the footer.

Two points handled deliberately rather than glossed over:

- Factory **visits and test drives are encouraged by appointment**, but general public factory
  tours are not offered. The About page's location section says so plainly.
- **Safety** is framed as the passive and active improvements over the original 1960s cars —
  which is what the FAQ supports. No page claims current FMVSS compliance.

### One open item

The hub (`index.html`) canonicalises to `https://revologycars.com/company/`. That path does not
exist yet — it is a proposal. If the client wants the hub somewhere else (`/inside-revology/`,
`/the-company/`), change the `<link rel="canonical">`, the `og:url`, the three `@id`/`url`
values in its JSON-LD, and the `/company` rewrite in `vercel.json`. The nav and footer link to
the hub as `/`, which does not change.
