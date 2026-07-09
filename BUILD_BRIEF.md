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
| Real class/studio photography | FB/IG photos noted as existing in LEADS.md but not retrieved this pass (see Design Notes) — do not fabricate substitutes |
| Any specific address/postcode | Site lists venue names only, no full addresses scraped this pass |

## Asset Manifest
| File | Source URL | Native size | License/credit | Watermark checked | Intended section | Copy match |
|---|---|---|---|---|---|---|
| (none used) | — | — | — | — | — | — |

**Final decision (2026-07-09, QA-completion pass): ship text/CSS-only, no photography.**
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
- Image layout pattern: N/A — final decision is text/CSS-only, see Asset Manifest.
- Risk notes: no imagery is the main gap vs. a normal build in this pipeline; site leans on typography, a subtle CSS "stage curtain" motif, and the alumni-placement fact for visual/narrative interest instead. This is a considered final decision, not an oversight — see Asset Manifest for the full sourcing attempt.

## Builder QA
- Contrast: PASS — 0 violations on all 3 pages at desktop (1280px) and mobile (375px); see QA_REPORT.md
- Upscale mobile/tablet/desktop: N/A — confirmed zero `<img>` tags across all 3 pages (grep + live DOM check)
- Broken images: N/A — no images placed
- Manual checks: complete — see QA_REPORT.md
