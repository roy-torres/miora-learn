# Learn your Metabolic Code: a UX design concept for MIORA Illinois

**Live site:** https://roy-torres.github.io/miora-learn/

A one-page, plain-language guide that helps a first-time visitor understand MIORA's "Metabolic Code" (70+ blood biomarkers grouped into five triads), what AI does with that data, and what to ask before booking.

> **Independent portfolio concept.** Not affiliated with, endorsed by, or published by Life Time or MIORA. Facility details come from MIORA's public Illinois page. Triad names and lessons are illustrative. Not medical advice.

## 30-second summary

- **What:** A single-page guide that explains MIORA's blood-based "Metabolic Code" and how AI is used, for skeptical first-time visitors.
- **How I worked:** Set design standards first (4px grid, 48px buttons, WCAG AA), measured the page against them, audited it against *UX for AI*, then revised it using an interview with a MIORA Wellness Coordinator.
- **Biggest decisions:** Honest limits up front (the method is labeled "unproven"); two clearly different calls to action for two kinds of visitors; plain wording to ease the fear of being judged or up-sold.
- **Measured fixes:** Text contrast 4.12:1 to 5.27:1; header button 40px to 48px; fonts self-hosted and a Content-Security-Policy added.
- **Limits:** One interview, no usability testing with real visitors. "Get Started" is a stand-in link.
- **Where to look:** [Process table](#how-i-worked) for the steps, [interview changes](#what-the-interview-changed) for the evidence behind the design, [`tasks/todo.md`](tasks/todo.md) for the full log.

---

This README is the story of how the design came together, in the order the decisions were made. The full working log, with checklists and measurements, is in [`tasks/todo.md`](tasks/todo.md).

---

## The design problem

MIORA sells elective, blood-based health testing. Two things make that hard to design for:

1. **Visitors are skeptical.** Someone whose labs are already "normal" can feel up-sold when told to go further.
2. **The product leans on AI and an unproven method.** The Metabolic Code is MIORA's own framework. The page has to explain it without over-promising.

So the goal was a page that is easy to read, honest about limits, and clear about what to do next.

## How I worked

I set the standards first, then built, then audited against them, then revised using a real conversation with someone who works there.

| Step | What I did | What it changed |
|---|---|---|
| 1. Set the rules | Wrote project rules: 4px spacing grid, 48px buttons, WCAG AA, and two reference books (a UI design book and *UX for AI* by Greg Nudelman). | Every later decision had something to be checked against. |
| 2. Accessibility pass | Measured instead of guessing. The header button was 40px (rule: 48px). The tertiary text color failed AA at 4.12:1 on the page background and 3.65:1 on the grey band. | Button to 48px. Text color darkened to 5.27:1 and 4.66:1. I also fixed a code comment that claimed a ratio it didn't meet. |
| 3. Ship early | Deployed to GitHub Pages so I could review the real page. | Found a typography problem I hadn't seen locally (next row). |
| 4. Typography | Heading letters ("rn" in "Learn") nearly touched at 79px. I tried tracking `-.015em`, then `-.005em`, then `0`, previewing each. | Settled on `0` with kerning on. |
| 5. Audit against *UX for AI* | Reviewed the page against the book. Strengths: AI "reads the pattern," people decide; flags are amber, not red; the evidence section admits the method is unproven. I found six gaps. | See "What the AI audit found" below. |
| 6. Interview | Interviewed a MIORA Wellness Coordinator at Life Time Evanston. | See "What the interview changed" below. |
| 7. Check a decision against Laws of UX | Two identical "Book a consultation" buttons stacked on mobile. | See "A small decision, argued from evidence" below. |
| 8. Security review | Audited the code for secrets and exploitable code. | Self-hosted fonts, added a Content-Security-Policy, removed `innerHTML`. |

## What the AI audit found

The page is about AI, not an AI product, so chat-style patterns don't apply. The book's trust and transparency chapters do. Six gaps, six small text-only fixes:

- **No data or consent information** → added questions to ask: who sees my results, how long they're kept, can I opt out of AI analysis or delete my data.
- **Nothing on what happens when the AI is wrong** → added a "When AI gets it wrong" list: it can miss or over-flag, your provider checks every flag, you can ask for a second look.
- **Capability claims stated as fact while the evidence section said "unvalidated"** → noted that the claims are in MIORA's own words and linked to the evidence.
- **Mockups looked like real results** → captioned "Sample data, not a real result."
- **"Optimal range" not qualified by age, sex, or background** → added a question about who the ranges were built from.
- **No path to raise a concern** → added "Who do I contact if I disagree with a result?"

## What the interview changed

I interviewed a Wellness Coordinator who works with MIORA members. I mapped each note to the page before changing anything:

| What I heard | What changed |
|---|---|
| Members think MIORA is "the food pyramid," but the dietitian looks at your lifestyle ("Let's pick two things a week you can change." "Are you the meal planner for your family?"). | New section, "Built around your week, not a food pyramid," using those phrases. |
| Members fear the dietitian will reprimand them. | Added plain wording that nobody is grading your plate. |
| It's elective, starts with a questionnaire, and produces a ~50-page report in five sections. | "What your visit includes" list. |
| People in the normal range suspect an up-sell. | A glossary entry that names the doubt and says going further is a judgment call, plus two new questions to ask. |
| MIORA wants to be proactive, not reactive; medicine shouldn't be one-size-fits-all. | The hero now opens with that idea. |
| Two audiences need two buttons: "Get Started" (already purchased) and "Book a consultation" (a talk). The coordinator felt both needed to be more prominent. | Both are 56px in the hero with a one-line helper saying who each is for. They are different colors so they don't compete. |

Two judgment calls worth noting:

- **Interview facts are labeled as such.** The questionnaire, the report length, and the one-hour review aren't on MIORA's public page, so the page says they come from a conversation with the Evanston team.
- **I didn't name a competitor.** The interview compared MIORA to another testing service. The page uses a general line instead.

## A small decision, argued from evidence

On mobile, the header's "Book a consultation" sat directly above an identical hero button. I checked [lawsofux.com](https://lawsofux.com/). It has **no rule about duplicate buttons**, so this is my reading of nearby principles, not a citation:

- *Von Restorff Effect*: key actions should stand out, and emphasized elements shouldn't compete.
- *Hick's Law*: fewer choices means faster decisions.
- *Cognitive Load*: unnecessary elements add load.

Decision: hide the header buttons on mobile, keep them on desktop where the header is far from the hero. The header isn't sticky, so no always-visible button was lost.

## Security notes

- No secrets or personal data in the repo or its git history. No forms, cookies, storage, analytics, or network calls.
- Fonts are self-hosted (Instrument Sans and Newsreader, SIL Open Font License), so visitors' IP addresses aren't sent to Google.
- A `<meta>` Content-Security-Policy allows only same-origin fonts and styles and pins the one inline script by SHA-256 hash. I tested it: an injected script didn't run and an outside request was blocked.
- Known limit: a `<meta>` policy can't set `frame-ancestors`, and GitHub Pages doesn't allow custom headers, so the page can still be framed by other sites.

## What this is not

Being clear about limits is part of the design approach:

- **One interview, no usability testing.** Nothing here has been tested with real visitors.
- **"Get Started" is a stand-in.** It links to MIORA's Illinois page because MIORA's own buttons there are placeholders (`#`). Swap in a real onboarding URL.
- **"Book a consultation" calls the clinic's public number.** A booking page may exist; I haven't confirmed one.
- **Medical claims are MIORA's, not mine.** The evidence section labels each one *Well established*, *Mixed or limited*, or *Emerging or unproven*, with sources.

## Possible next steps

- Test the page with a handful of first-time visitors, especially people in the "normal range."
- Confirm a real onboarding URL and booking path.
- Add a sticky mobile call-to-action if testing shows people lose the buttons while scrolling.

## Run it locally

No build step. Any static server works:

```sh
python3 -m http.server 8765
# then open http://localhost:8765
```

If you edit the inline `<script>` in `index.html`, recompute its SHA-256 hash and update the Content-Security-Policy `<meta>` tag, or the interactive ring will stop working.

## Files

- `index.html`: the whole page (HTML, CSS, one small script)
- `fonts/`: self-hosted WOFF2 fonts
- `tasks/todo.md`: the plan, measurements, and review notes for every change
- [`claude.md`](claude.md): the project rules (design standards, workflow, security)

Built with [Claude Code](https://claude.com/claude-code) as a working partner. The research, interview, and design decisions were mine.
