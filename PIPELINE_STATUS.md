# Pipeline Status — Hampstead Ballet School

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: DEPLOY — complete. Site is live. Next step is outreach drafting
  (top-level session's job — see Outreach state below).
- Last trusted commit: `c3a7e58` ("QA fix pass: contrast, mobile nav bleed-through,
  horizontal overflow"), on top of `47b4614` ("Initial build"). Working tree is clean
  — nothing uncommitted as of this handoff.
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
  - Imagery: retried sourcing `banner-website.jpg` and `home_img.jpg` — the earlier
    "TLS reset" was confirmed transient, both downloaded fine this pass, but both were
    rejected on content grounds (dated banner graphic; 200x215px stock-looking photo).
    Checked Facebook (unclaimed "Unofficial Page", 0 posts) and Instagram (real but
    stale account, full photos gated behind a login this pipeline won't perform).
    **Final decision: ship text/CSS-only.** This is documented in BUILD_BRIEF.md as a
    closed decision, not an open TODO.
- Next exact action:
  1. Deploy — DONE (this pass). Public GitHub repo `Plainset/hampstead-ballet-school-demo`
     created, `master` pushed (matches repo default branch), Pages enabled
     (`source[branch]=master`, `source[path]=/`), build confirmed `status: built`,
     and all 3 pages independently confirmed loading (HTTP 200 + correct `<title>`
     on each of `/`, `classes.html`, `contact.html`) via `curl`.
  2. Remaining: draft outreach per AGENTS.md step 7 (email to
     `info@hampsteadballetsch.co.uk`, leading with the observation that the current
     2017-era site undersells the alumni-placement prestige story) — top-level
     session's job this round, not this repo's. Log it in `OUTREACH_LOG.md`
     (top-level session's job, not this repo's).
  3. Optional, non-blocking: consider sourcing/confirming the "Finchley Road" detail
     for the O2 Centre venue into `BUILD_BRIEF.md`'s Allowed Facts, or trim it back to
     "O2 Centre" — see QA_REPORT.md REVIEW-phase Verdict for detail. Does not block
     deploy.
- Deploy URL: **https://plainset.github.io/hampstead-ballet-school-demo/** — confirmed
  live 2026-07-09: GitHub Pages build status `built`, all 3 pages (`/`, `classes.html`,
  `contact.html`) return HTTP 200 with correct page titles.
- Outreach state: none sent yet — not drafted from this repo this pass (top-level
  session is drafting directly). Not in scope for this phase's edits.
- Flags for Alex:
  - This build ships with no photography at all, by considered final decision (see
    BUILD_BRIEF.md). The alumni-placement prestige angle (Royal Ballet School,
    Vaganova, etc.) carries the visual/narrative interest instead.
  - While inspecting the school's own current banner image for reuse, it surfaced a
    phone number ("07764 587 851") baked into that graphic. This is **not** verified
    as current/live and was **not** added to BUILD_BRIEF.md's Allowed Facts or used
    anywhere on the demo site (independently confirmed absent via grep during REVIEW)
    — flagging only in case it's useful context for outreach later.
  - Email `info@hampsteadballetsch.co.uk` remains the only verified contact channel.
  - Do not touch `LEADS.md`/`OUTREACH_LOG.md` from this repo; the top-level session
    owns those.
