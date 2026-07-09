# QA Report — Hampstead Ballet School

## Pages Checked
- index.html (partial — contrast audit only, before time box ran out)
- classes.html (not yet checked)
- contact.html (not yet checked)

## Audit Results
| Check | Result | Evidence |
|---|---|---|
| Contrast audit (index.html) | FAIL found, then FIXED | `contrast-audit.js` via preview_eval on http://localhost:4219/ found 4 violations: `.brand small`, `.card .num` (x3) all using `--rose` (#c98a86) at ratio 2.52-2.81 vs threshold 4.5. Fixed by adding darker `--rose-deep: #953a35` (contrast ~7.1:1 on white) and repointing all rose-on-light text uses (`.brand small`, `.card .num`, `.venue-pill .tag`, `.contact-list strong`, `details.faq summary::after`) to it. Not re-run after fix — see Blocking Issues. |
| Contrast audit — additional manual finding | FAIL found, then FIXED | Manual luminance calc found `--gold` (#b98c4c, used only by `.section-head .eyebrow` on light backgrounds) at ~3.0:1, below the 4.5:1 threshold for its ~11.5px non-bold text. Darkened to `--gold: #8a611f` (~5.5:1 calculated). Not re-verified live — see Blocking Issues. |
| Upscale mobile | N/A | No images placed this pass (see BUILD_BRIEF.md Asset Manifest — homepage image downloads failed with repeated TLS resets during the build window) |
| Upscale tablet | N/A | same as above |
| Upscale desktop | N/A | same as above |
| Broken images | N/A | No `<img>` tags in the built pages |
| Aspect mismatch advisory | N/A | No images |

## Manual Checks
| Check | Result | Notes |
|---|---|---|
| Text on photo | N/A | No photos used |
| Gradient/::before backgrounds | Needs manual check | `contrast-audit.js` flagged `a.cta-ghost` on `.hero`'s linear-gradient background as `needsManualCheck` (script can't resolve gradients). Visually: cream text (#f7f2ea) on a dark curtain gradient (#23070d-#4a1c28) — high contrast by inspection, but not machine-verified. |
| Image/content match | N/A | No images |
| Fabricated claims | PASS (self-check) | All facts trace to BUILD_BRIEF.md Allowed Facts table, sourced from live WebFetch of hampsteadballetsch.co.uk on 2026-07-09. No named individual alumni, no prices/term dates invented — see Do Not Claim table. |
| Mobile layout | NOT CHECKED | Time box ran out before mobile-width pass. CSS mobile breakpoint (`@media max-width:780px`) is in place (nav toggle, stacked hero) per the shared pattern in `AGENTS.md` but not visually verified this pass. |

## Blocking Issues
| Issue | Evidence | Required fix |
|---|---|---|
| Contrast fixes not re-verified live | `--rose-deep` and darkened `--gold` were edited in style.css after the audit ran, but the contrast-audit script was not re-run against the live page afterward (time box) | Re-run `contrast-audit.js` via preview_eval against all 3 pages after these CSS edits; confirm 0 violations before deploy |
| classes.html and contact.html never audited | Only index.html was run through contrast-audit.js | Run contrast-audit.js against classes.html and contact.html; both reuse the same `--rose`/`--rose-deep`/`--gold` classes flagged/fixed on index.html, so likely same-shape issues, but not confirmed |
| Mobile viewport check not done | Not attempted this pass | Resize to ~375px width and check nav toggle, hero, venue-scroll, and contact-card layouts on all 3 pages |
| Preview tool returned innerWidth/innerHeight: 0 mid-session | `getBoundingClientRect()` and `offsetWidth` returned 0 for real, laid-out elements (`offsetHeight` was non-zero) after both default load and an explicit `preview_resize` call — looks like a preview-harness viewport bug, not a site bug, but not fully diagnosed | Whoever resumes: try a fresh `preview_start` (new serverId) before re-running audits; if the 0-width issue persists, it's a tooling problem to flag, not something to "fix" in the site |

## Advisory Issues
- No real photography placed yet (see BUILD_BRIEF.md) — biggest gap vs. the finished pitch. Follow-up pass should retry sourcing `banner-website.jpg`/`home_img.jpg` from the live site and the school's FB/IG (owner-uploaded photos only).

## Fixed Verification
| Issue | Fix | Recheck result |
|---|---|---|
| `--rose` text failing AA (2.52-2.81:1) on `.brand small`, `.card .num` | Added `--rose-deep: #953a35`, repointed 5 text-color usages | NOT RE-CHECKED LIVE (time box) — hand-calculated ~7.1:1 on white |
| `--gold` eyebrow text failing AA (~3.0:1) on `.section-head .eyebrow` | Darkened `--gold` to `#8a611f` | NOT RE-CHECKED LIVE (time box) — hand-calculated ~5.5:1 on white |

## Verdict
BLOCKED — build is functionally complete (3 pages, sourced copy, no fabricated facts) but QA is incomplete: contrast fixes were made but not re-verified live, classes.html/contact.html were never audited, and no mobile-width pass was done. Do not deploy until the Blocking Issues above are cleared.
