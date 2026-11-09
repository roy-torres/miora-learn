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
