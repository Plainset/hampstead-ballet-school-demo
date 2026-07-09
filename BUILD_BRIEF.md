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

No photographic assets used this pass. Direct download of the two homepage images
(banner-website.jpg, home_img.jpg) from hampsteadballetsch.co.uk failed repeatedly with
TLS "Connection reset by peer" (curl exit 35) during the build window — likely
server-side throttling from concurrent agent traffic in this batch run. Real FB/IG class
photos mentioned in LEADS.md were not attempted this pass due to the 12-minute time box.
Site built as clean typographic/CSS design (no fabricated or mismatched stock photos),
per AGENTS.md guidance to cut images rather than force a mismatch. **Follow-up:** a later
pass should retry pulling banner-website.jpg / home_img.jpg and the school's own FB/IG
class photos (owner-uploaded only) and slot them into the hero and gallery sections.

## Design Notes
- Palette: deep navy/blackboard (#1a1f2e family) + warm blush/rose gold accent (#d9a5a0 family) + cream (#f7f3ee) — evokes stage curtains and pointe-shoe satin rather than generic pastel-pink "kids dance" cliche, fits the prestige/serious-training angle.
- Image layout pattern: N/A this pass (no images) — CSS defaults (aspect-ratio patterns) documented for future asset drop-in.
- Risk notes: no imagery is the main gap; site leans on typography, a subtle CSS "stage curtain" motif, and the alumni-placement fact for visual/narrative interest instead.

## Builder QA
- Contrast: pending (see QA_REPORT.md)
- Upscale mobile: N/A — no images placed
- Upscale tablet: N/A — no images placed
- Upscale desktop: N/A — no images placed
- Broken images: N/A — no images placed
- Manual checks: pending (see QA_REPORT.md)
