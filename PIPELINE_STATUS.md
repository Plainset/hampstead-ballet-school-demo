# Pipeline Status — Hampstead Ballet School

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: BUILD — complete. QA_REPORT.md verdict is PASS. Ready to hand off to REVIEW.
- Last trusted commit: see git log in this repo. A new commit is still needed after this
  handoff covering: `--gold` darkening, `.contact-card .eyebrow` fix, the `.site-header`
  backdrop-filter/containing-block fix, `overflow-x:hidden` on html/body, `overflow-wrap`
  on `.contact-card`, and this documentation update (BUILD_BRIEF.md, QA_REPORT.md, this
  file) — see Next exact action.
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
  1. Review the diff in `style.css` (six small, targeted changes — see QA_REPORT.md
     Fixed Verification table for the full list) and commit it in this repo.
  2. Hand off to REVIEW phase per `.pipeline/checklists/REVIEW.md` — an independent
     reviewer should still spot-check the blocking-issue fixes rather than take this
     report on faith, per AGENTS.md's "a reviewer may use the report as a map, but
     still verifies blocking checks independently."
- Deploy URL: none — **do not deploy from BUILD phase**. No GitHub repo/Pages created
  yet. Deployment is a REVIEW/FIX+DEPLOY+DRAFT-phase action, not in scope here.
- Outreach state: none — no email drafted or sent. Not in scope for this phase.
- Flags for Alex:
  - This build ships with no photography at all, by considered final decision (see
    BUILD_BRIEF.md). The alumni-placement prestige angle (Royal Ballet School,
    Vaganova, etc.) carries the visual/narrative interest instead.
  - While inspecting the school's own current banner image for reuse, it surfaced a
    phone number ("07764 587 851") baked into that graphic. This is **not** verified
    as current/live and was **not** added to BUILD_BRIEF.md's Allowed Facts or used
    anywhere on the demo site — flagging only in case it's useful context for outreach
    later, since the original build pass had recorded "no phone number listed."
  - Email `info@hampsteadballetsch.co.uk` remains the only verified contact channel.
  - Do not touch `LEADS.md`/`OUTREACH_LOG.md` from this repo; the top-level session
    owns those.
