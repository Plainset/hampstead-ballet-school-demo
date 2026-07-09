# Pipeline Status — Hampstead Ballet School

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: BUILD (time-boxed to ~12 minutes; stopped for handoff, not finished)
- Last trusted commit: see git log in this repo (first commit, made right after this file was written)
- Known untrusted state:
  - Contrast fixes (`--rose-deep`, darkened `--gold`) were made in `style.css` but **not re-verified live** with `contrast-audit.js` after the edit.
  - Only `index.html` was ever run through the contrast audit; `classes.html` and `contact.html` were never audited (likely share the same fixed/unfixed classes, but unconfirmed).
  - No mobile-width (~375px) visual check was done on any page.
  - No images were sourced or placed this pass — direct downloads of the two homepage images from hampsteadballetsch.co.uk failed repeatedly with TLS "Connection reset by peer" (curl exit 35), and FB/IG photos mentioned in LEADS.md were never attempted due to the time box. Site currently ships as clean typography/CSS only, no photography.
  - Mid-session, `preview_eval` returned `innerWidth`/`innerHeight: 0` and `getBoundingClientRect().width: 0` for real, correctly-laid-out elements (offsetHeight was non-zero), even after an explicit `preview_resize` call. This looks like a preview-harness bug in this session, not a site bug — a fresh `preview_start` should be tried before trusting further audit results.
- Next exact action:
  1. `preview_start` a fresh server for `hampstead-ballet-school-site` (port 4219, already registered in `.claude/launch.json`).
  2. Re-run `.pipeline/qa/contrast-audit.js` via `preview_eval` against `index.html`, `classes.html`, and `contact.html`; confirm 0 violations. If violations remain on `--rose-deep`/`--gold`, adjust further.
  3. Do a real narrow-width (~375px) pass on all 3 pages: nav toggle opens/closes, hero doesn't overflow, `.venue-scroll` scrolls, `.contact-card` doesn't clip.
  4. Decide on imagery: retry curl/WebFetch of `banner-website.jpg`/`home_img.jpg` from hampsteadballetsch.co.uk, and check the school's public Facebook/Instagram for owner-uploaded class photos (per AGENTS.md step 3 rules — no third-party/watermarked images). If nothing usable turns up quickly, it's fine to ship text-only; record that decision in BUILD_BRIEF.md either way.
  5. Update `QA_REPORT.md` Verdict from BLOCKED to PASS once the above is done, then this business is ready for the REVIEW phase (not this session's job).
- Deploy URL: none — **do not deploy**, per BUILD-phase rules. No GitHub repo/Pages created yet.
- Outreach state: none — no email drafted or sent. Not in scope for this phase.
- Flags for Alex:
  - This build currently ships with **no photography at all** — the biggest gap vs. a normal build in this pipeline. The prestige alumni-placement angle (Royal Ballet School, Vaganova, etc.) carries the pitch instead, but real class photos would strengthen it materially.
  - Email `info@hampsteadballetsch.co.uk` was re-confirmed live via WebFetch on 2026-07-09 immediately before this build — still good.
  - Do not touch `LEADS.md`/`OUTREACH_LOG.md` from this repo; the top-level session owns those.
