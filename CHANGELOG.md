# Polish Pass — Changelog

Scope of this pass: Accessibility, SEO, semantics, JS/code quality, dead-file
cleanup (per your priority selection). No visual redesign — same structure,
same design language, same features.

## Fixed

**SEO**
- `<title>` was hardcoded in English while `<html lang="ru">` and the rest of
  the meta tags were already Russian — now consistent.
- Added `canonical`, `og:url`, `og:image`, `robots`, `theme-color`, full
  Twitter Card tags, and a JSON-LD `Person` schema.
- ⚠️ The canonical URL and `og:image`/`twitter:image` currently point to a
  placeholder domain (`alexsokolov.photography`) — replace with your real
  domain before publishing, or these tags will be actively wrong.

**Accessibility**
- Contact form: replaced border-color-only validation (invisible to screen
  readers and colorblind users) with real error messages (`role="alert"`),
  `aria-invalid`, `aria-describedby`, and focus moving to the first invalid
  field on failed submit. Error copy follows the active site language.
- Mobile menu button: `aria-expanded` was set once on page load and never
  updated on toggle — now stays in sync, and the accessible label switches
  between "Открыть меню" / "Закрыть меню".
- Added `rel="noopener noreferrer"` to the WhatsApp/Telegram links, which
  used `target="_blank"` without it (a real security gap, not just a lint
  nit — the linked page could otherwise access `window.opener`).

**Performance**
- Google Fonts request trimmed from 7 weights to the 5 actually used in the
  CSS (300/400/500/600/700) — 800/900 were dead weight.
- Added `width`/`height` to portfolio thumbnails to reduce layout shift.

**Code quality / maintainability**
- Removed two dead files: a one-line stub `translations.js` and a second,
  *differently structured and entirely unused* `css/translations.js` — the
  real translation data lives in `index.html`'s second `<script>` block.
  Three copies of "the same" data in one project is exactly the kind of
  thing a reviewer flags first.
- Removed two leftover debug `console.log` statements.
- Consolidated the `window.__alexPortfolioData` / `window.__alexOpenLightbox`
  ad-hoc globals into a single `window.AlexPortfolio` namespace object.
- Removed several inline `style="..."` attributes that either duplicated an
  existing CSS rule verbatim (dead code) or belonged in the stylesheet —
  moved to proper classes (`.about-avatar-img`, `.detail-links`, form error
  states).

## Verified after the changes
- Both `<script>` blocks still parse without syntax errors.
- The JSON-LD block is valid JSON.
- No duplicate element IDs.
- All major tags (`div`, `section`, `header`, `main`, `footer`, `form`,
  `button`) are balanced.
- No remaining references to the removed translation files.

## Not done in this pass (out of scope / flagged, not fixed)
- No visual/CSS redesign — you asked for polish, not a rebuild.
- The ~2,150-line single `<style>` block wasn't split into logical sections
  — that's a bigger structural change than "fix the bug," worth a separate
  pass if you want it.
- Contact info (phone/WhatsApp/Telegram/email) is left as-is — that's your
  content, not a code issue.
- Responsive layout wasn't stress-tested across breakpoints beyond reading
  the existing media queries.

Let me know if you'd like a follow-up pass on any of the above, or on the
other audit categories (CSS architecture split, deeper performance work,
full responsive QA).
