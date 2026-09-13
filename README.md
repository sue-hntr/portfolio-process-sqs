# Design Token System — suzanne-hunter.com

A foundational, non-destructive design-token pass for the Squarespace 7.1 site,
built in `typography-tokens.css` and pasted into Design → Custom CSS.

- **Phase 1 (typography):** an 8-level type hierarchy as CSS custom
  properties, applied to Squarespace's native `h1`–`h6` tags plus two opt-in
  utility classes.
- **Phase 2 (color):** a cool, teal-tinted color primitive + semantic token
  system. Tokens only — not yet wired to any selector beyond what Phase 1
  already used (see below).

No layout, spacing, content, or button-component styling is touched.

## Install

1. Squarespace admin → **Design → Custom CSS**.
2. Paste the entire contents of [`typography-tokens.css`](typography-tokens.css)
   in (append below anything already there, or replace if this is the whole
   custom stylesheet).
3. Save. Changes apply immediately — no template edits needed.
4. To use the two custom classes on specific elements: select the text/button
   in the Squarespace editor, open its block settings, and add `text-link` or
   `text-cta` in the **Custom CSS Class** field.

## What it does

- Declares font tokens (`--font-serif` → Ovo, `--font-sans` → Nunito Sans) and
  a full color token system (primitives + semantic layer — see Phase 2 below)
  for reuse.
- Styles the native `h1`–`h6` tags sitewide (font, size, weight, line-height,
  letter-spacing, margin-bottom) — works across all existing content blocks
  automatically.
- Adds two opt-in utility classes for text that has no native heading tag:
  `.text-link` (inline text links) and `.text-cta` (button/CTA-style text
  labels), each with hover/focus states.
- Keeps selector specificity low (single tag or single class, no IDs, no
  `!important`) so any style you set explicitly on a block in the Squarespace
  editor still wins.

## The scale, in plain terms

Base size is 16px (1rem), matching the current site. Sizes step up using a
**1.25 ratio** ("Major Third"), rounded to clean rem values:

| Level | Element | Typeface | Size | Role |
|---|---|---|---|---|
| H1 | `h1` | Ovo (serif) | 3rem / 48px | Page / hero title |
| H2 | `h2` | Ovo (serif) | 2rem / 32px | Major section title |
| H3 | `h3` | Nunito Sans | 1.5rem / 24px | Subsection title |
| H4 | `h4` | Ovo (serif) | 1.25rem / 20px | Card / project title |
| H5 | `h5` | Nunito Sans | 0.875rem / 14px | Eyebrow / small subhead (uppercase) |
| H6 | `h6` | Nunito Sans | 0.75rem / 12px | Metadata / caption |
| L7 | `.text-link` | Nunito Sans | 1rem / 16px | Text link |
| L8 | `.text-cta` | Nunito Sans | 0.875rem / 14px | Button/CTA label (uppercase) |

**Why this ratio:** at 1.25×, the scale lands within ~1px of your site's
current live H1 (~48px) and H2 (~32px) sizes, so it reads as continuous with
what's already there rather than a visible jump.

**Why H5/H6/CTA break the strict curve:** eyebrow text, captions, and CTA
labels conventionally read as hierarchy through *uppercase + letter-spacing*,
not through shrinking further on a pure ratio — that would hurt legibility
for no real gain. Their letter-spacing does that work instead.

## Color tokens (Phase 2)

Cool, teal-tinted neutrals, chosen after comparing warm/cool/true-gray
options side by side. Two layers:

**Primitives** — raw color scales, not used directly in components:

| Scale | Steps |
|---|---|
| Magenta | 100, 200, 500, 700, 900 |
| Teal | 100, 300, 600, 900 |
| Cool neutral | 50, 100, 300, 600, 900 |

**Semantic** — what components should actually reference:

| Token | Resolves to | Use |
|---|---|---|
| `--color-background` / `--color-background-alt` | cool-50 / cool-100 | Page/section surfaces |
| `--color-border` | cool-300 | Dividers, outlines |
| `--color-text-body` / `--color-text-muted` | cool-900 / cool-600 | Body copy / secondary text |
| `--color-text-link` / `--color-text-link-hover` | teal-900 / magenta-900 | Inline text links |
| `--color-primary` / `--color-primary-hover` | magenta-900 / magenta-700 | Brand primary |
| `--color-secondary` / `--color-secondary-hover` | teal-900 / teal-600 | Brand secondary |
| `--color-cta-bg` / `--color-cta-bg-hover` / `--color-cta-text` | magenta-900 / magenta-700 / white | Primary CTA/button |
| `--color-cta-secondary-bg` / `-hover` / `-text` | teal-900 / teal-600 / white | Secondary CTA/button |

**Contrast confirmed (no adjustments made to the provided values):**
white text on `--magenta-900` is 8.48:1 (passes AAA); white text on
`--teal-900` is 5.45:1 (passes AA, not AAA).

**Not yet applied anywhere new.** These tokens don't touch any selector this
pass — that's deliberate, saved for CTA prototyping next. The one real side
effect: `--color-primary`/`--color-secondary` already existed as Phase 1
placeholders (raw hex, "reference only"). `--color-primary` is unchanged
(`#891085`). `--color-secondary` shifts from the old placeholder `#008080`
to the chosen `--teal-900` (`#007580`) — a small shift that `.text-link:hover`
and `.text-cta:hover` (already live from Phase 1) pick up automatically,
since they reference `var(--color-secondary)`.

## Flag for a future pass

Nunito Sans's proportions (x-height, width) tend to read a touch small at a
16px body size compared to serif-forward body copy. Worth considering a
site-wide bump to 17–18px base (Design → Fonts → Paragraph) later — **not**
changed in this pass, since it's scoped to the token hierarchy, not the
global base size.

## Dev tooling (not part of the Squarespace deliverable)

`static-server.mjs` and `preview.html` are local-only files used during Claude
Code work sessions to visually review the type scale in a browser before
anything gets pasted into Squarespace. They're not meant to be uploaded,
linked, or referenced anywhere on the live site — only `typography-tokens.css`
goes into Design → Custom CSS.

## Out of scope (by design)

- Color tokens are declared but not wired to any new selector — only the
  Phase 1 `.text-link` / `.text-cta` hover states use them (see above).
  Applying colors to backgrounds, borders, and CTAs is the next phase.
- Section spacing / global padding — untouched.
- Squarespace's native button component styling (the actual `.sqs-button`
  system) — untouched. `.text-cta` is a standalone text style, not a button
  override.
