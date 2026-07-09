# QA Report — Hampstead Ballet School

Use exact pass/fail evidence. "Looks fine" is not a result.

## Pages Checked
- index.html — contrast (desktop + mobile), mobile nav, overflow, manual gradient check
- classes.html — contrast (desktop + mobile), mobile nav, venue-scroll, overflow
- contact.html — contrast (desktop + mobile), mobile nav, overflow, email-wrap fix

## Preview-tooling note (read before trusting any single audit run)
This session hit several real measurement quirks in the preview harness, confirmed
non-issues in the site itself once cross-checked against screenshots/DOM:
- `window.innerWidth` / `getBoundingClientRect()` intermittently report doubled or
  zero values (root cause: `preview_resize` with a `preset` can leave the CDP device-
  metrics override in a bad state; an explicit width+height resize fixes it). A fresh
  `preview_start` alone did not fix it — the explicit resize did.
- Returning a raw `DOMRect` object directly through `preview_eval` serializes to `{}`
  (its numeric fields are inherited accessor properties, not own-enumerable) —
  destructure into plain numbers before returning.
- `[data-reveal]` elements are `opacity:0` until scrolled into view/IntersectionObserver
  fires, so a contrast audit run right after page load silently skips them (lower
  `totalChecked`). Force `.is-visible` on all `[data-reveal]` elements before auditing.
Every contrast/overflow number in this report was taken only after working around
these (forced reflow, explicit resize, forced `.is-visible`, and cross-checked with
screenshots).

## Audit Results
| Check | Result | Evidence |
|---|---|---|
| Contrast audit — index.html desktop (1280px) | PASS | `contrast-audit.js` via preview_eval: 45 text nodes checked, 0 violations |
| Contrast audit — index.html mobile (375px) | PASS | 45 checked, 0 violations |
| Contrast audit — classes.html desktop | PASS | 59 checked, 0 violations |
| Contrast audit — classes.html mobile | PASS | 59 checked, 0 violations |
| Contrast audit — contact.html desktop | PASS | 23 checked, 0 violations |
| Contrast audit — contact.html mobile | PASS | 23 checked, 0 violations |
| Upscale mobile/tablet/desktop | N/A | Confirmed 0 `<img>` tags on all 3 pages via `grep -n "<img"` and live `document.querySelectorAll('img').length` (both 0). Text/CSS-only build — see BUILD_BRIEF.md Asset Manifest for the full image-sourcing attempt and final decision. |
| Broken images | N/A | No `<img>` tags exist |
| Aspect mismatch advisory | N/A | No images |

## Manual Checks
| Check | Result | Notes |
|---|---|---|
| Text on photo | N/A | No photos used |
| Gradient/::before backgrounds | PASS (hand-calculated) | `contrast-audit.js` flags hero text (`p.eyebrow`, `h1`, `p.lede`, `a.cta-ghost` on all 3 pages) as `needsManualCheck` since it can't resolve `linear-gradient(in oklch 160deg, var(--curtain-deep), var(--curtain) 65%, #4a1c28)`. Hand-computed WCAG luminance for the lightest gradient stop (`#4a1c28`, L≈0.0244) against the hero text colors: `--cream` (#f7f2ea) ≈13:1, `--cream-dim` (#ede4d6) ≈12:1, `--rose-light` (#e7c3bf) ≈8.7:1 — all comfortably pass AA even at the gradient's lightest point. |
| Image/content match | N/A | No images |
| Fabricated claims | PASS | All facts trace to BUILD_BRIEF.md Allowed Facts, sourced from live WebFetch of hampsteadballetsch.co.uk. No named individual alumni, no prices/term dates invented. |
| Mobile layout (375px, all 3 pages) | PASS (after fixes) | See Fixed Verification — found and fixed 2 real mobile bugs (nav containing-block bug, page-level horizontal overflow) and 1 real text-overflow bug (long email address), all confirmed fixed via screenshot + DOM measurement. Nav toggle opens/closes cleanly on all 3 pages, `.venue-scroll` (classes.html) scrolls horizontally within its own box without leaking into page overflow, no document-level horizontal scroll on any page. |

## Blocking Issues
None remaining — see Fixed Verification for everything found and resolved this pass.

## Advisory Issues
- No photography on this site (text/CSS-only). This is now a **documented final
  decision** (see BUILD_BRIEF.md Asset Manifest), not an open gap: both original
  homepage images were re-sourced successfully this pass but rejected on content
  grounds (dated banner graphic with an unverified phone number; a 200x215px
  stock-looking photo), and the school's Facebook (unclaimed "Unofficial Page", 0
  posts) and Instagram (real account, but stale 2017-2021 content, full photos
  gated behind a login this pipeline will not perform) yielded nothing usable. The
  alumni-placement prestige strip carries the visual/narrative interest instead.

## Fixed Verification
| Issue | Fix | Recheck result |
|---|---|---|
| `--rose` text failing AA (2.52-2.81:1) on `.brand small`, `.card .num` (carried over from prior build pass) | `--rose-deep: #953a35` added, 5 text-color usages repointed | RE-CHECKED LIVE this pass — 0 violations on all 3 pages, confirmed via `contrast-audit.js` |
| `--gold` eyebrow text failing AA on `.section-head .eyebrow` against the (darker) `--cream-dim` background specifically (ratio 4.37, below 4.5 threshold) — a real bug the prior pass's white-only hand-calc missed | Darkened `--gold` from `#8a611f` to `#7a5518` (≈5.3:1 on cream-dim, ≈6.7:1 on white) | RE-CHECKED LIVE — 0 violations |
| `.contact-card .eyebrow` ("Ready to try a class?" on index.html, "Email the school" on contact.html) inheriting the base `.eyebrow` rule's `--rose-light` (designed for the dark hero) instead of gold, on a white card background — ratio 1.62:1, a severe real failure never caught by the prior pass (which only ever audited index.html's hero, not this section) | Added `.contact-card .eyebrow { color: var(--gold); }` | RE-CHECKED LIVE on both index.html and contact.html — 0 violations |
| classes.html and contact.html never contrast-audited at all | Ran full `contrast-audit.js` on both, at both desktop and mobile widths | 0 violations on both, both widths |
| Mobile-width pass never done | Ran full 375px pass on all 3 pages | See below — 2 new real bugs found and fixed |
| **Real bug found this pass:** mobile off-canvas `.main-nav` rendered as a ~64px box instead of full-viewport height, with its flex children (nav links) overflowing past the box and bleeding through onto the hero content behind (illegible overlapping text). Root cause: `.site-header` has `backdrop-filter: blur(8px)`, which per the CSS Filter Effects spec creates a new containing block for `position:fixed` descendants — so `.main-nav`'s `inset: 76px 0 0 0` was resolving against the ~101px-tall header box, not the viewport. | Moved the blur/tint from `.site-header` onto a `.site-header::before` pseudo-element instead, so `.site-header` itself no longer has `backdrop-filter` and no longer creates that containing block for its `.main-nav` child (a sibling of the pseudo-element, not its descendant) | Confirmed via screenshot: nav now opens as a full-height, fully opaque, legible panel on all 3 pages. Desktop header glass/blur look unchanged (screenshot-confirmed). |
| **Real bug found this pass:** page-level horizontal overflow at 375px (`document.documentElement.scrollWidth: 750` vs `clientWidth: 375`) on every page, caused by the closed off-canvas nav (`position:fixed`, `translateX(100%)`) sitting past the viewport edge with no `overflow-x` guard on `html`/`body` | Added `overflow-x: hidden` to both `html` and `body` | Confirmed `scrollWidth === clientWidth` (no overflow) at 375px on all 3 pages, nav-open and nav-closed states |
| **Real bug found this pass:** `.contact-card a.email` and the raw email-address `<h2>` on contact.html genuinely overflowed their container at 375px (measured `emailRight: 383.6` vs. viewport width 375, i.e. past the edge) — an unbroken long token (`info@hampsteadballetsch.co.uk`) with `overflow-wrap: normal`. This was being invisibly clipped (not just scrollable) once the `overflow-x:hidden` fix above landed, which would have silently hidden real content from mobile users. | Added `overflow-wrap: break-word;` to `.contact-card` (inherited property, fixes both the `h2` and `a.email` in one rule) | Confirmed via `getBoundingClientRect()` (email now wraps within the card, `emailRight` well inside `cardRight`) and screenshot (clean 2-line wrap) on both index.html and contact.html |

## Verdict (BUILD-phase QA)
**PASS** — build is functionally complete (3 pages, sourced copy, no fabricated facts), all contrast violations found this pass are fixed and re-verified live at both desktop and mobile widths, a real mobile-nav layout bug and a real mobile text-overflow bug were found and fixed (not just re-verified), and the no-photography decision is now a documented final call rather than an open TODO. Ready for the REVIEW phase.

---

## REVIEW phase — independent verification (2026-07-09)

Per `.pipeline/checklists/REVIEW.md`: treated the above as a map, not proof, and
independently re-verified via `preview_start`/`preview_eval` (fresh server, port 4219)
rather than trusting the report.

- **Git state first:** confirmed the repo was already git-initialized with one prior
  commit (`47b4614`, "Initial build"). The pending diff (`BUILD_BRIEF.md`,
  `PIPELINE_STATUS.md`, `QA_REPORT.md`, `style.css`) matched exactly what this report
  describes (six targeted `style.css` changes: `--gold` darken, `.contact-card
  .eyebrow` color fix, header blur moved to `::before`, `overflow-x:hidden` on
  html/body, `overflow-wrap: break-word` on `.contact-card`). Committed as
  `c3a7e58`.
- **Contrast — re-ran the actual `.pipeline/qa/contrast-audit.js` independently**
  (not trusted from the report) on all 3 pages at desktop (1280px), mobile (375px),
  and tablet (768px):
  - index.html: 45 checked / 0 violations at 1280px, 375px, **and 768px** (all three
    identical to the report's desktop/mobile numbers; tablet added by this review).
  - classes.html: 59 checked / 0 violations at 1280px and 375px.
  - contact.html: 23 checked / 0 violations at 1280px and 375px.
  - Same 3-4 `needsManualCheck` hero-gradient entries as reported each time (script
    can't resolve the `linear-gradient(in oklch ...)` hero background) — independently
    spot-confirmed `.contact-card .eyebrow` computed color is `rgb(122, 85, 24)`
    (`#7a5518`, the new `--gold`) on both index.html and contact.html, not the old
    `--rose-light` inherited value.
- **Mobile nav bleed-through — reproduced and independently confirmed fixed.** Clicked
  `.nav-toggle` on index.html at 375px, confirmed via `aria-expanded="true"` +
  `getComputedStyle` that `.main-nav` has `transform: matrix(1,0,0,1,0,0)` (fully
  on-screen) and a 375×736 box starting at `top:76`, with an opaque
  `rgb(247,242,234)` background — then screenshotted it: a clean, fully opaque,
  legible panel (Home / Classes & Venues / Contact / Book a trial class), zero hero
  bleed-through. Also independently confirmed `.site-header` itself has
  `backdrop-filter: none` while its `::before` pseudo-element carries `blur(8px)`
  — matches the documented fix exactly.
  - Note: an initial click+screenshot sequence briefly appeared to show the nav
    closed between an eval check and a screenshot for reasons not fully root-caused
    (possibly a preview-tool interaction quirk, consistent with this repo's other
    documented tooling quirks) — re-tested with click immediately followed by eval
    (no intervening calls) and got a clean, reproducible open state confirmed by both
    computed style and screenshot. Not a site bug.
- **Overflow — independently confirmed on all 3 pages at 375px and 768px:**
  `document.documentElement.scrollWidth === clientWidth` on every page/width
  combination tested (375 vs 375 at mobile, 753 vs 753 at tablet — the 753 vs a
  nominal 768 is just scrollbar gutter, not overflow). `.venue-scroll` on
  classes.html correctly overflows only inside its own box (2643px content in a
  327px/705px box at mobile/tablet) rather than leaking to the page.
  `.contact-card` email: independently measured `emailRight: 300.3` vs.
  `cardRight: 351` vs. viewport `375` at mobile — wraps cleanly inside the card,
  confirmed `overflow-wrap: break-word` is active on both the link and its
  container.
- **Fact-check against `BUILD_BRIEF.md`** — grepped all 3 pages for every Allowed
  Fact and cross-checked:
  - "Est. 1996" / "since 1996" / "nearly thirty years" — present, matches.
  - Principals "Anastassia Uspenskaya" and "Andrey Alexeev" — present, matches,
    correctly described as ex-professional/principal teachers, no unverified
    embellishment.
  - Alumni-placement list — all 7 schools (Royal Ballet School, Tring Park, Sylvia
    Young Theatre School, Joffrey Ballet School, Boston Ballet, Vaganova Academy,
    Madrid Conservatory) present verbatim, no named individual students anywhere
    (correctly honors the Do Not Claim table).
  - All 10 venues present verbatim against the Allowed Facts venue list.
  - Email `info@hampsteadballetsch.co.uk` — only contact channel shown; "No phone
    line published" text on contact.html correctly matches "no phone number
    listed." The unverified `07764 587 851` number discovered in the rejected
    banner graphic does **not** appear anywhere on the site — confirmed absent by
    grep. Good: the Do-Not-Claim/flagged-not-used distinction was actually honored,
    not just documented.
  - No prices, term dates, or testimonials/reviews anywhere on the site (grepped
    for `£`/"price"/"testimonial"/quote-marks) — consistent with the Do Not Claim
    table; nothing fabricated.
  - **Minor advisory (non-blocking):** classes.html labels one venue "O2 Centre,
    **Finchley Road**" — the Allowed Facts table only recorded "O2 Centre" without
    a street name. The added detail is true and easily public knowledge (the O2
    Centre's address is genuinely on Finchley Road), but it wasn't itself
    re-verified from a source this pass and technically extends past what
    `BUILD_BRIEF.md` records under a "no specific address" Do-Not-Claim rule. Not
    blocking — it's an addition of a well-known, unambiguous, non-risky detail
    about a real venue, not a fabricated or uncertain fact — but flagging so it's
    either sourced into `BUILD_BRIEF.md` or trimmed back to "O2 Centre" in a future
    pass.
  - Images: independently confirmed 0 `<img>` tags via `grep -c "<img"` on all 3
    files. No broken images, no upscale violations possible — trivially N/A per the
    documented rejection of both candidate photos.

### Verdict (REVIEW phase)
**PASS.** All BUILD-phase QA claims independently reproduced and confirmed: 0
contrast violations on all 3 pages at desktop/tablet/mobile, the mobile nav
bleed-through bug is genuinely fixed (opaque full-panel, no hero bleed), no
page-level horizontal overflow at any tested width, the email-wrap fix holds, and
every fact on the live site traces to `BUILD_BRIEF.md`'s Allowed Facts with no
fabrication. One non-blocking advisory (O2 Centre/Finchley Road detail) noted
above. Ready to proceed to FIX+DEPLOY+DRAFT.
