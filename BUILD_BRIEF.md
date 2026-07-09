# Build Brief — Hampstead Ballet School

Keep this compact. Add only sourced facts and assets actually used or deliberately rejected.

## Contact
- Email: info@hampsteadballetsch.co.uk
- Email source URL: https://hampsteadballetsch.co.uk (homepage/footer, via WebFetch)
- Rechecked date: 2026-07-09 — confirmed still renders (WebFetch re-check same day as build)
- Phone: none found on site
- Address: no single address; ~10 hired venues (see Allowed Facts)

## Page Plan
- Scope: 3-page default
- Pages: index.html (Home), classes.html (Services/Offer — classes + venues), contact.html (Contact/Booking)
- Reason for any extra page: none added; venues folded into Classes page as a section since they're operationally part of "where to find a class," not a separate pitch angle

## Pitch Hook
- Verified observation: current site is a 2017-era WordPress build (footer reads "2017 © Hampstead Ballet School"), minimal mobile optimisation, no visual presence for a school whose alumni have gone on to Royal Ballet School, Vaganova Academy, Tring, Joffrey, Boston Ballet, Madrid Conservatory — a strong prestige story that the current site doesn't showcase at all.
- Source URL: https://hampsteadballetsch.co.uk (homepage text + footer, via WebFetch 2026-07-09)

## Allowed Facts
| Fact | Source URL | Used where |
|---|---|---|
| Teaching since 1996 | hampsteadballetsch.co.uk homepage | Home, About/hook |
| Principals: Anastassia Uspenskaya, Andrey Alexeev (ex-professional) | hampsteadballetsch.co.uk homepage | Home |
| Alumni placed at Royal Ballet School, Tring, Sylvia Young, Joffrey Ballet School, Boston Ballet, Vaganova Academy, Madrid Conservatory | hampsteadballetsch.co.uk homepage (quoted) | Home, Classes |
| Email info@hampsteadballetsch.co.uk | hampsteadballetsch.co.uk footer | Contact |
| No phone number listed | hampsteadballetsch.co.uk (site-wide check) | Contact |
| Venues: O2 Centre, St Andrew's URC, Quaker Friends Meeting House, The Rooms Above (West Heath Yard), Heathside Prep School, Armoury Hampstead, Hampstead Parish Church, Primrose Hill Community Centre, Mansergh Club, St Anthony's School for Girls | hampsteadballetsch.co.uk /venues/ | Classes |
| Classes offered for children/teenagers and adults, beginner to advanced | hampsteadballetsch.co.uk /classes/ | Classes |
| Site copyright reads "2017 © Hampstead Ballet School" | hampsteadballetsch.co.uk footer | internal note / pitch hook only, not shown on demo site |

## Do Not Claim
| Claim or uncertainty | Reason |
|---|---|
| Exact class prices/term dates | Not confirmed in this pass — omit, direct to email instead |
| Specific named alumni (individuals) | Only school-level placement claims quoted on source site; no named students found — do not invent |
| Real class/studio photography beyond the one now used | The `bwg_gallery` photos (see Asset Manifest) were not discovered until the 2026-07-09 fix pass; most are watermarked (`www.osawahorowitzmediaproductions.com`, a third-party recital photographer) and rejected per AGENTS.md's watermark rule — do not use any `DSC_*w.jpg`/`DSC_*01/02/03w.jpg`-style recital-stage files site-wide without re-checking each individually for that watermark first |
| Any specific address/postcode | Site lists venue names only, no full addresses scraped this pass |

## Asset Manifest
| File | Source URL | Native size | License/credit | Watermark checked | Intended section | Copy match |
|---|---|---|---|---|---|---|
| `images/barre-practice.jpg` | `https://hampsteadballetsch.co.uk/wp-content/uploads/photo-gallery/09072017-01.jpg` (school's own WP photo gallery, `bwg_gallery` plugin, found via `wp-sitemap-posts-bwg_gallery-1.xml`) | 800x600 | Business's own site, own gallery upload; no third-party credit visible | Checked individually — full frame and top/bottom crops inspected, no watermark text anywhere in the image (unlike the `DSC_*` recital photos in the same gallery, which carry a `www.osawahorowitzmediaproductions.com` photographer credit — see rejected list below) | Home, hero (2-column layout beside the intro copy) | Real students at the ballet barre doing pointe work in a studio (stained-glass windows, mirrors) — directly matches "ballet training" |

**2026-07-09 fix pass: added real photography (client feedback).** A prior pass's
"ship text/CSS-only" decision below is superseded for the homepage hero only — the
rest of that reasoning (no photos elsewhere on the site) still stands.

- Client feedback (verbatim, relayed by Alex): homepage "seems a bit mean-looking...
  no images at all... it'd be nice to see a picture." Re-opened the asset search
  rather than treating the prior "final decision" as unchangeable.
- Found a WordPress photo gallery (`bwg_gallery` plugin) on the school's own site,
  not discovered in the original build pass — reachable via
  `https://hampsteadballetsch.co.uk/wp-sitemap-posts-bwg_gallery-1.xml` →
  `/bwg_gallery/main/`. Contains ~30 real photos across two visual groups:
  1. Recital/stage photos (filenames `DSC_NNNNw.jpg`/`s.jpg`) — most carry a visible
     `www.osawahorowitzmediaproductions.com` watermark in-frame (a third-party
     photographer/videographer credit) and were rejected per AGENTS.md's
     third-party-watermark rule. A few individual files in this set
     (`DSC_8603s.jpg`, `DSC_8613s.jpg`, `DSC_8670s.jpg`) happened to have no visible
     watermark on inspection, but were still not used — reserved as a documented,
     not-yet-used fallback if a second photo is wanted later, since the "ballet
     training" ask is better matched by a class/studio shot than a stage
     performance shot.
  2. Two studio/class photos (`09072017-01.jpg`, `09072017-02.jpg`, 800x600, no
     watermark, filename pattern distinct from the photographer's `DSC_*` series —
     consistent with these being uploaded directly by the school, not the hired
     recital photographer). Both inspected full-frame and via top/bottom-band crops
     — no watermark or credit text found in either.
- Selected `09072017-01.jpg` (four students at the barre doing pointe work, church-
  hall studio with stained-glass windows and mirrors) for the homepage hero: real,
  on-brand, directly answers "a form of ballet training," and its warm pink-toned
  wall complements the rose/blush accent already in the palette.
- Not re-checked/used this pass: `09072017-02.jpg` (also clean, could be a second
  future asset for e.g. the Classes page) and the 3 unwatermarked `DSC_*s.jpg`
  recital shots noted above.

## Superseded 2026-07-09 (BUILD-phase) decision — kept for history
**Original final decision (2026-07-09, QA-completion pass): ship text/CSS-only, no photography.**
This was re-attempted properly this pass, not left on the earlier TLS excuse:

- The earlier "TLS Connection reset by peer" was confirmed transient — `curl` succeeded
  this pass on the first retry. Both original homepage images were located (the
  earlier build guessed the wrong path; the real paths are under
  `/wp-content/uploads/2017/06/`) and downloaded successfully:
  - `banner-website.jpg` (1100x200, real resolution) — inspected and REJECTED. It is
    the school's actual dated 2017 WordPress banner graphic: a flashy gradient-text
    logo reading "Hampstead Ballet School" with a phone number "07764 587 851" baked
    into the image, plus keyword text ("Children from 2+ / Teenagers / Adults all
    levels"). This is a banner/logo graphic, not a photo, and using it would visually
    reproduce the exact dated look the pitch hook is built around replacing — self-
    defeating for a redesign demo. (Note: the phone number visible in this graphic is
    NOT independently verified as current/live — do not add it to Allowed Facts or
    Contact without a fresh confirmation; flagged here only as a discovery, not a claim.)
  - `home_img.jpg` — only 200x215 native pixels. Far too small to display anywhere on
    a modern site without failing the upscale audit (any use above ~260px width
    already exceeds the 1.3x gate), and its content (a professional-looking pas de
    deux stage pose) reads more like stock/press photography than a verified photo of
    this school's own students — no clear content-match confirmation was possible at
    this size. Rejected per AGENTS.md's "cut rather than force a mismatch" guidance.
- Checked the business's own social accounts (the only other AGENTS.md-sanctioned
  source): Instagram (@hampstead_ballet_school, found via the homepage) has a public
  post grid visible without login (50 posts, 203 followers) but the account is
  stale — last visible activity is a New Year's greeting graphic, with the bulk of
  real content dated 2017-2021. Individual/full-resolution photos require an
  Instagram login to open, which is a prohibited action (no account creation/login)
  — so no full-res file could be retrieved from IG this pass. Facebook
  (facebook.com/pages/Hampstead-Ballet-School/193090200706496) turned out to be an
  unclaimed "Unofficial Page" auto-generated listing: 0 followers, "No posts yet" —
  no content exists there at all.
- No usable, rights-clear, on-brand photo asset could be retrieved through any
  AGENTS.md-sanctioned channel after this real attempt. Site ships as the existing
  clean typographic/CSS design (stage-curtain gradient motif, alumni-placement
  prestige strip) with zero `<img>` tags — confirmed via QA_REPORT.md. This is a
  final decision for this build, not an open follow-up: a future pass could revisit
  only if someone obtains an Instagram login or the school shares photos directly.

## Design Notes
- Palette: deep navy/blackboard (#1a1f2e family) + warm blush/rose gold accent (#d9a5a0 family) + cream (#f7f3ee) — evokes stage curtains and pointe-shoe satin rather than generic pastel-pink "kids dance" cliche, fits the prestige/serious-training angle.
- Image layout pattern (2026-07-09 fix pass): hero split into a `.hero-grid` (text left, photo right on desktop/tablet; text first, photo below on mobile via `order`), photo at `aspect-ratio: 4/3`, `object-fit: cover`, rounded/bordered to match the site's `--radius` and card-border language. Reason: client feedback that the homepage "seems a bit mean-looking... no images at all" — the hero is the first thing seen, so this is the highest-impact single placement for one photo, directly softening the curtain-dark palette without touching contrast (photo sits beside the text, not behind it).
- Risk notes: only one photo is used site-wide (homepage hero); classes.html and contact.html remain text/CSS-only by design (no verified, on-brand, unwatermarked asset was matched to their content this pass) — not an oversight, just scoped to what the client actually flagged.

## Builder QA
- Contrast: PASS — 0 violations on all 3 pages at desktop (1280px) and mobile (375px); re-confirmed 2026-07-09 fix pass with the hero photo in place (46 checked / 0 violations, desktop + mobile) — see QA_REPORT.md
- Upscale mobile/tablet/desktop: PASS on the one image now placed (`images/barre-practice.jpg`, 800x600 native) — 0 violations at 375px/768px/1280px, rendered well under the 1.3x gate at all three. classes.html/contact.html remain N/A (zero `<img>` tags).
- Broken images: PASS — 0 `brokenImages` (naturalWidth/Height confirmed nonzero via live DOM)
- Manual checks: complete — see QA_REPORT.md
