# Pipeline Status — Hampstead Ballet School

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: FIX+DEPLOY — complete, client feedback addressed and redeployed.
  Next step is outreach drafting (top-level session's job — see Outreach state
  below).
- Last trusted commit: see `git log` — this pass adds one commit on top of
  `cd714b1` ("Deploy: live on GitHub Pages, confirmed all 3 pages loading") for
  the 2026-07-09 client-feedback fix pass (empty placements row + homepage
  photo). Working tree clean after that commit.

## 2026-07-09 — client feedback fix pass
Real feedback from Alex after reviewing the live site, two issues, both fixed
and redeployed this pass:
1. **Empty bottom row in the alumni-placements section** — root cause: CSS
   Grid `auto-fit` reserving empty cells in the last row when 7 real items
   didn't divide evenly into the computed column count (6 at desktop widths).
   Fixed by switching `.placements` to `flex-wrap` so the last row's item(s)
   grow to fill it instead of leaving empty cells. No data was missing/removed
   — all 7 alumni-placement facts are still shown, verified against
   `BUILD_BRIEF.md` Allowed Facts.
2. **No images on the homepage** — found a previously-unchecked photo gallery
   on the school's own site (`bwg_gallery` WordPress plugin, via
   `wp-sitemap-posts-bwg_gallery-1.xml`). Most photos there are watermarked by
   a third-party recital photographer (`www.osawahorowitzmediaproductions.com`)
   and rejected per AGENTS.md's watermark rule; two (`09072017-01.jpg`,
   `09072017-02.jpg`) had no watermark. Used `09072017-01.jpg` (real students
   at the barre, pointe work, studio setting) as `images/barre-practice.jpg` in
   a new two-column hero layout. See `BUILD_BRIEF.md` Asset Manifest for full
   sourcing detail and `QA_REPORT.md` for the fix-pass verification (contrast:
   0/46 violations at 1280px+375px; upscale: 0 violations/broken/mismatches at
   375/768/1280px; overflow unaffected).
- State as of this pass:
  - All contrast violations found (both carried-over and newly discovered) are fixed
    and re-verified live via `contrast-audit.js` on all 3 pages at both 1280px and
    375px — 0 violations everywhere. See QA_REPORT.md for full detail.
  - Two real mobile-only bugs were found and fixed, not just re-verified:
    1. The off-canvas mobile nav (`.main-nav`) was rendering as a broken ~64px box
       instead of a full-screen panel, due to `backdrop-filter` on `.site-header`
       creating an unintended CSS containing block for its fixed-position child.
       Fixed by moving the blur onto a `.site-header::before` pseudo-element.
    2. Genuine page-level horizontal overflow at 375px from the closed off-canvas nav
       (no `overflow-x:hidden` guard existed). Fixed, and separately caught + fixed a
       long-email-address text overflow that the overflow-x fix would otherwise have
       silently clipped (`overflow-wrap: break-word` added to `.contact-card`).
  - A preview-tooling quirk was investigated, not left as a mystery: `window.innerWidth`
    / `getBoundingClientRect()` intermittently read 0 or doubled — root-caused to
    `preview_resize` with a `preset` leaving the CDP device-metrics override in a bad
    state; an explicit width+height resize call fixes it. Documented in QA_REPORT.md
    for the next agent so it isn't re-litigated.
  - Imagery (historical, superseded 2026-07-09 fix pass): earlier passes retried
    sourcing `banner-website.jpg` and `home_img.jpg` (both rejected on content
    grounds) and checked Facebook/Instagram (nothing usable), landing on a
    "ship text/CSS-only" decision. **This is now superseded for the homepage
    hero**: the 2026-07-09 fix pass found the school's own `bwg_gallery` photo
    gallery (not checked in earlier passes) and added one real, unwatermarked
    photo — see the client-feedback section above and `BUILD_BRIEF.md` Asset
    Manifest for full detail. classes.html/contact.html remain text/CSS-only
    (unchanged, not part of this feedback).
- Next exact action:
  1. Deploy — DONE. Public GitHub repo `Plainset/hampstead-ballet-school-demo`
     live at the URL below; the 2026-07-09 fix-pass commit (empty-row fix +
     homepage photo) has been pushed and Pages rebuilt — confirmed loading with
     both fixes live (see below).
  2. Remaining: draft outreach per AGENTS.md step 7 (email to
     `info@hampsteadballetsch.co.uk`, leading with the observation that the current
     2017-era site undersells the alumni-placement prestige story) — top-level
     session's job this round, not this repo's. Log it in `OUTREACH_LOG.md`
     (top-level session's job, not this repo's).
  3. Optional, non-blocking: consider sourcing/confirming the "Finchley Road" detail
     for the O2 Centre venue into `BUILD_BRIEF.md`'s Allowed Facts, or trim it back to
     "O2 Centre" — see QA_REPORT.md REVIEW-phase Verdict for detail. Does not block
     deploy.
  4. Optional, non-blocking: `09072017-02.jpg` (also a clean, unwatermarked studio
     photo from the same gallery) and 3 unwatermarked recital shots
     (`DSC_8603s.jpg`, `DSC_8613s.jpg`, `DSC_8670s.jpg`) are documented in
     `BUILD_BRIEF.md` as available but unused — a future pass could add one to
     classes.html if a second image is ever wanted there.
- Deploy URL: **https://plainset.github.io/hampstead-ballet-school-demo/** — confirmed
  live 2026-07-09 after the fix-pass push: GitHub Pages rebuilt, homepage shows
  the corrected placements table (no dangling empty row) and the new barre-practice
  photo in the hero; all 3 pages still return HTTP 200 with correct page titles.
- Outreach state: none sent yet — not drafted from this repo this pass (top-level
  session is drafting directly). Not in scope for this phase's edits.
- Flags for Alex:
  - Both pieces of feedback (empty placements row; no homepage photo) are fixed
    and live as of 2026-07-09 — see the client-feedback section above for root
    cause and verification detail.
  - The homepage now has one real photo (students at the barre, from the
    school's own photo gallery). classes.html and contact.html remain
    text/CSS-only, by scope, not oversight — nothing in this feedback asked for
    photos there.
  - While inspecting the school's own current banner image for reuse (an
    earlier pass), it surfaced a phone number ("07764 587 851") baked into that
    graphic. This is **not** verified as current/live and was **not** added to
    BUILD_BRIEF.md's Allowed Facts or used anywhere on the demo site (confirmed
    absent via grep) — flagging only in case it's useful context for outreach
    later.
  - Email `info@hampsteadballetsch.co.uk` remains the only verified contact channel.
  - Do not touch `LEADS.md`/`OUTREACH_LOG.md` from this repo; the top-level session
    owns those.
