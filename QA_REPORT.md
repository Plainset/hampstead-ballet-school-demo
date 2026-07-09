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

## Verdict
**PASS** — build is functionally complete (3 pages, sourced copy, no fabricated facts), all contrast violations found this pass are fixed and re-verified live at both desktop and mobile widths, a real mobile-nav layout bug and a real mobile text-overflow bug were found and fixed (not just re-verified), and the no-photography decision is now a documented final call rather than an open TODO. Ready for the REVIEW phase.
