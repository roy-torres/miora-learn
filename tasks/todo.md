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

---

# Revise UI from Vanessa interview (Wellness Coordinator, MIORA Evanston)

Source: interview notes. Page stays an independent concept, so interview facts are worded as "what members are told," not official MIORA claims. Text-first edits; small CSS only for the CTA change. Needs your OK before work starts.

## What Vanessa said -> what it means for the page
| Note | Current page | Gap |
|---|---|---|
| Members think MIORA is "the food pyramid"; dietitian looks at lifestyle ("pick two things a week", "are you the meal planner for your family?") | `#how` is clinical: blood draw, plan, support | Lifestyle-fit side is missing |
| Members fear the dietitian will reprimand them | Nothing on tone of visit | Fear isn't addressed |
| Elective questionnaire, 50-page report, 5 sections, each with its own triad | Hero says "five triads"; no report or questionnaire | Report and questionnaire not shown |
| "Normal range" people feel up-sold when told to go further | `Normal versus optimal` lesson, Evidence section | Skepticism not named; no "why go further" answer |
| Proactive, not reactive; not one-size-fits-all | Hero lede is neutral | Core message not stated |
| Function Health: blood draw at facility, then 1-hour specialist review | No comparison | Differentiator unclear (see question 2) |
| Two CTAs: "Get Started" (already purchased) and "Book a Consultation" (sit-and-talk). Need more prominence | Header has small "Book a consultation"; hero has "Start learning" and "Visit Evanston"; no "Get Started" | Two audiences, weak CTAs |

## Plan
- [ ] 1. **CTAs (Vanessa's main ask).** Hero: replace "Start learning" / "Visit Evanston" with a primary "Book a consultation" and a second solid "Get Started" (for people who've already purchased), each with a one-line helper under it saying who it's for. Keep "Start learning" as a text link. Header: keep one 48px "Book a consultation" and add "Get Started". Both 48px+, AA contrast, visibly different from each other.
- [ ] 2. **Proactive, personal line.** Add one sentence to the hero lede: care that starts before something is wrong and is built around you, not a standard plan.
- [ ] 3. **Lifestyle section** (new, short, in `#how` or just after): "It's about your life, not a food pyramid." Two example questions in Vanessa's words, plus "Pick two things a week you can change." Tone: no judgment.
- [ ] 4. **Fear of being reprimanded.** One line under that section: the dietitian's job is to fit changes into your week, not to grade your food choices.
- [ ] 5. **What you get.** Add to `#how`: a short questionnaire (elective), a roughly 50-page report, five sections each with its own triad. Link the existing triad ring to this.
- [ ] 6. **"Isn't normal good enough?"** Add a short answer near the "Normal versus optimal" lesson and in the Questions list: why a provider might suggest going further, and how to ask "what would change if I did this?" This stays consistent with the Evidence section's honesty about limits.
- [ ] 7. **Function Health.** Hold until you answer question 2.
- [ ] 8. Check contrast (AA), 4px grid, 48px buttons, mobile (390px) and desktop; screenshot; fill in Review; commit and push.

## Questions for you
1. **"Get Started" destination:** where should it go? Options: a login or onboarding link (give me the URL), `tel:` like the call button, or a placeholder `#` that I flag as unfinished.
2. **Function Health:** do you want a comparison on the page (naming a competitor on a page that says it's independent), or just a general line like "some services draw blood and hand you a report; MIORA adds a conversation with a specialist"? Also, do the notes mean MIORA does the one-hour specialist review, or that Function Health does? I read it as MIORA's contrast, but I'm unsure.
3. **"Book a Consultation"** currently calls a phone number. Keep that, or is there a booking page?
4. **Quotes:** OK to use Vanessa's phrases verbatim as sample prompts (unattributed), or should I reword them?

## Decisions (after your answers)
- "Get Started" -> https://miora.lifetime.life/location/illinois.html (MIORA's own Get Started and Book buttons there are `#` placeholders; no real purchase URL exists). Flag as stand-in in Review.
- "Book a consultation" keeps `tel:8474489724`. Ask Vanessa if a booking page exists.
- Function Health: no competitor named. General line plus the one-hour review with Krupesh Patel, FNP-BC, nurse practitioner (title as on MIORA's public page; interview-only titles not used).
- One-hour review, questionnaire, 50-page report come from the interview only, not MIORA's site. Word as "your visit includes" and note the source.
- Vanessa's phrases may be used verbatim, unattributed.

## Progress (edits made in index.html, NOT yet committed or pushed)
- [x] 1. CTAs: hero now has "Book a consultation" (tel) and "Get Started" (MIORA Illinois page), both 56px with a one-line helper; "Start learning" became a text link. Header has both at 48px; "Get Started" hides on mobile to avoid crowding.
- [x] 2. Hero lede opens with the proactive, personal line.
- [x] 3+4. New "Built around your week, not a food pyramid" section with Vanessa's two phrases and a no-grading line.
- [x] 5+7. "What your visit includes" list in `#how` (optional questionnaire, ~50-page report in five sections, about an hour with Krupesh Patel, FNP-BC), a general line instead of naming Function Health, and a source note that these details come from the interview.
- [x] 6. Glossary entry "Taking it to the next level" and two new questions about the normal range.
- [x] 8a. Static checks: white on Get Started 4.79:1, white on Book 16.26:1, helper text 7.83:1, source note 4.66:1 (all AA); tags balanced; spacing uses the 4px scale.
- [x] 8b. Browser check at 1280px and 390px via a local server you started: no horizontal overflow; hero buttons 56px, header buttons 48px; mobile header shows only Book; new sections render cleanly. Widened CTA helper text (240px -> 280px) to stop one-word wraps.
- [x] 8c. Commit and push, then Review.

## Review
- CTAs are now the most prominent thing in the hero: "Book a consultation" (tel, for people who want to talk) and "Get Started" (for people who already purchased), 56px, with helper text. Header has both at 48px.
- Added the messages from the interview: proactive and personal care, lifestyle-first dietitian visits (Vanessa's two phrases), no-judgment tone, what the visit includes, and an honest answer to the "is this an upsell?" worry.
- Function Health is not named; one general contrast line is used instead.
- Interview-only facts (questionnaire, ~50-page report, one-hour review) are labeled as such on the page.
- Open items: "Get Started" points to MIORA's Illinois page because MIORA's own buttons there are `#` placeholders; swap in a real onboarding URL when you have one. Ask Vanessa whether a booking page exists, and whether Krupesh Patel can be named publicly.

## Follow-up: duplicate "Book a consultation" on mobile
- Laws of UX has no rule on repeated buttons. Closest: Von Restorff (key actions distinct, emphasis shouldn't compete), Hick's Law (fewer choices), Cognitive Load (unnecessary elements).
- Change: on screens 860px and narrower, both header buttons are hidden (one CSS line). Mobile keeps the hero buttons and the bottom "Call to book a consultation". Desktop header unchanged.
- The header is not sticky, so no persistent mobile CTA is lost.
