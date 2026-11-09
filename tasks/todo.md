# Todo: Bring index.html in line with claude.md rules

Scope: small CSS-only edits in `index.html`. No layout or content rewrites.

## Findings (measured)
- `.btn.sm` (header CTA) was 40px tall. Rule: buttons 48px.
- `--ink-3` (#6B7C88) was 4.12:1 on paper and 3.65:1 on the fog band. WCAG AA needs 4.5:1 for small text. The CSS comment claiming 4.6:1 was wrong.
- 12px paddings are on the 4px grid (3 x 4), so they are allowed. Left unchanged.
- Already fine: focus ring, skip link, reduced-motion, `.btn` 48px, link colors, `lang`, landmarks.

## Plan
- [x] 1. Make `.btn.sm` 48px (`min-height:48px`), keep its smaller horizontal padding and font.
- [x] 2. Darken `--ink-3` to #5A6B77 and fix the comment.
- [x] 3. Re-measure contrast and screenshot desktop and mobile to check the header still fits at 48px.
- [~] 4. Skipped by choice: 12px paddings are already on the 4px grid.
- [x] 5. Fill in the Review section below.

## Review
- `.btn.sm`: `min-height` 40px -> 48px. Header looks right at 1280px and 390px wide.
- `--ink-3`: #6B7C88 -> #5A6B77. New contrast: 5.27:1 on paper, 4.66:1 on the fog band (both pass AA 4.5:1). Comment corrected.
- Same color swapped in three hard-coded spots in the inline SVG mockups (two small text labels, one outline) so they match.
- Not changed: 12px paddings, font sizes, layout, content.
- Only console message while testing was a missing favicon (harmless). Test server stopped and screenshots deleted.

---

# Todo: Deploy to GitHub Pages

Account: `roy-torres` (gh CLI logged in, `repo` scope). Free Pages needs a **public** repo, so the page and repo contents become public.

- [x] 1. Add `.gitignore` (`.env`, `.env.*`, `!.env.example`, `.claude/settings.local.json`, `.claude/logs/`, `.playwright-mcp/`, plus the three `preview-*.png` files, which the site doesn't use).
- [x] 2. `git init`, commit `index.html`, `claude.md`, `tasks/`, `.gitignore` (branch `main`).
- [x] 3. Scan staged files with `git diff --cached` for secret patterns. Stop if anything shows. (Pre-check on current files: clean.)
- [x] 4. `gh repo create roy-torres/miora-learn --public --source=. --push`.
- [x] 5. Enable Pages from `main` branch, `/` root, via `gh api`.
- [x] 6. Wait for the build, then check the URL loads (expected `https://roy-torres.github.io/miora-learn/`).
- [x] 7. Add a Review section here.

## Review
- `.gitignore` added (secrets, `.playwright-mcp/`, `preview-*.png`). Staged-diff secret scan was clean.
- Committed `index.html`, `claude.md`, `tasks/`, `.gitignore` to `main` and pushed to a new public repo: https://github.com/roy-torres/miora-learn
- GitHub Pages enabled from `main` `/`. Build finished and the site returns HTTP 200.
- Live: https://roy-torres.github.io/miora-learn/
- To update: edit, `git commit`, `git push`; Pages rebuilds in about a minute.

---

# Todo: Fix heading tracking and kerning

- [x] Change `h1,h2,h3` letter-spacing `-.015em` -> `-.005em`, add `font-kerning:normal`.
- [x] Re-screenshot `h1` (1280px) and an `h2`; check mobile (390px) for overflow.
- [x] Commit and push so the live page updates.

## Review
- Cause: uniform -1.19px tracking on the 79px `h1` stacked on Newsreader's tight display cut, so straight-stem pairs ("rn" in "Learn", "kn", "ow") nearly touched.
- Fix: one rule in `index.html`. Tracking is now -.005em (about -0.4px at 79px), kerning explicitly on. Applies to `h1`, `h2`, `h3`.
- Checked: "Learn" letters separate, `h2` even, no horizontal overflow at 390px. If still too tight, try `0`.
- Follow-up: "rn" in "Learn" still looked close, so tracking set to `0` (kerning stays on). Previewed locally, then approved.

---

# Audit: index.html vs. "UX for AI" (Nudelman)

Read-only. Page is a static education page about AI, not an AI product. Copilot, restating, autocomplete, regen and similar patterns (ch. 7-9) don't apply. Page refs are the PDF's page numbers.

## Passes
- **Right use case / augmented intelligence** (ch. 2 p43, ch. 23 p462): The "AI reads the pattern, people make the decision" section keeps AI from presuming to tell experts their job.
- **Human confirms before acting** (ch. 5 value matrix, ch. 9 p183): Flags say "worth a conversation," not a verdict. Cost of a false flag is low.
- **Emotional safety** (p438): Amber, not red, for flags. Calm wording.
- **Honest limits** (p436-437): Evidence section labels the Metabolic Code "Emerging or unproven" and says no independent validation was found.
- **Breaking literacy barriers** (p437): Plain language, glossary, short lessons.
- **User autonomy** (p435): "Questions to ask before you book" gives users control.

## Gaps (most important first)
- [x] A. **No data/consent info** (p435-436, 438): The page is about reading 70+ personal health markers with AI, but says nothing on who sees results, retention, deletion, or opting out of AI analysis.
- [x] B. **No "what if the AI is wrong"** (p46, p436 "Ownership of mistakes"): AI section never says AI can be wrong, or how a provider catches it or how a patient disagrees. The book says plan for "when (not if)" the AI makes mistakes.
- [x] C. **Capability claims vs. evidence** (p436 reliability/explainability): "What AI does" states abilities as fact (compares 70+ markers, spots hidden combinations) while Evidence says it's unvalidated. Reader may overtrust.
- [x] D. **Mockups not labeled as sample data** (p436 transparency): The lab-report and phone mockups show "Vitamin D below your optimal zone" and a ring with an amber flag; only the footer says "illustrative."
- [x] E. **Bias/representation not mentioned** (p437): "Optimal range" isn't qualified by age, sex, or background, and the page doesn't ask who the model/ranges were built on.
- [x] F. **No path to raise a concern** (p437 accountability): Only a booking phone number.

## Proposed fixes (small, text-only, no layout change; need your OK)
- [x] A+E+F: Add 3 questions to the existing "Questions to ask before you book" list: who sees my results and how long they're kept, can I opt out of AI analysis or delete my data, and were the ranges built from people like me (age, sex, ethnicity). Plus one for "who do I contact if I disagree with a result?"
- [x] B: Add a third short list under "AI reads the pattern..." titled "When AI gets it wrong" (2-3 bullets: it can miss or over-flag; your provider checks every flag against how you feel; you can ask for a second look).
- [x] C: Add "according to MIORA's own description; see Evidence" to the "What AI does" heading or a one-line note.
- [x] D: Add a "Sample data, not a real result" caption under both mockups (14px+, AA contrast).
- [x] Re-check contrast, 4px grid, and mobile; screenshot; update Review; commit and push.

## Review
- Questions list: added 4 (who sees results and retention; opt out or delete; ranges built from people like me; who to contact if I disagree). Covers gaps A, E, F.
- AI section: added a "When AI gets it wrong" list (3 bullets, amber markers, full width via `.limits.solo`) for gap B, and a one-line note that the AI claims are in MIORA's own words, linking to the Evidence section, for gap C.
- Mockups: wrapped in `<figure>` with 14px captions "Sample data, not a real result." and "Sample plan, not a real patient." for gap D. Contrast of the grey caption text is 4.66:1+ on both backgrounds.
- CSS added: `.limits.solo` and `figcaption` (2 lines). No other styling changed.
- Checked at 1280px and 390px: layouts clean, no horizontal overflow.
- Not done: guardrails/restating etc. (no AI input on the page). If a chat or search is ever added, define denied requests first (ch. 9).
- The page makes no claims about MIORA's data practices; it only asks the questions.
