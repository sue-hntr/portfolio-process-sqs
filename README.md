# Design Token System — suzanne-hunter.com

A foundational, non-destructive design-token pass for the Squarespace 7.1 site,
built in `typography-tokens.css` and pasted into Design → Custom CSS.

- **Phase 1 (typography):** an 8-level type hierarchy as CSS custom
  properties, applied to Squarespace's native `h1`–`h6` tags plus two opt-in
  utility classes.
- **Phase 2 (color):** a cool, teal-tinted color primitive + semantic token
  system. Tokens only — not yet wired to any selector beyond what Phase 1
  already used (see below).
- **Phase 3 (spacing):** an 8-step modular spacing scale, applied via one
  native tag (`p`), one native structural selector (`section.page-section`),
  and two opt-in classes for card padding/gaps.

No content, block structure, or button-component styling is touched. Phase 3
is the first pass to touch spacing (paragraph rhythm + section padding).

## Install

1. Squarespace admin → **Design → Custom CSS**.
2. Paste the entire contents of [`typography-tokens.css`](typography-tokens.css)
   in (append below anything already there, or replace if this is the whole
   custom stylesheet).
3. Save. Changes apply immediately — no template edits needed.
4. To use the opt-in custom classes on specific elements (`text-link`,
   `text-cta`, `card-padding`, `card-grid-gap`): select the element in the
   Squarespace editor, open its block settings, and add the class name in
   the **Custom CSS Class** field.

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
| `--white` | `#FFFFFF` — used for CTA text on dark backgrounds |

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

## Spacing tokens (Phase 3)

8-step modular scale, 4px base, rem units — sized for a content-driven
portfolio site, not a dense data interface:

| Token | Value | Suggested use |
|---|---|---|
| `--space-1` | 0.25rem / 4px | Tight inline spacing |
| `--space-2` | 0.5rem / 8px | Small gaps, icon spacing |
| `--space-3` | 1rem / 16px | Default paragraph/element spacing |
| `--space-4` | 1.5rem / 24px | Card padding, related-element grouping |
| `--space-5` | 2rem / 32px | Spacing between distinct components |
| `--space-6` | 3rem / 48px | Spacing between major content blocks |
| `--space-7` | 4rem / 64px | Section padding (top/bottom) |
| `--space-8` | 6rem / 96px | Major section breaks, hero spacing |

**Applied to:**

| Selector | Rule | Type |
|---|---|---|
| `p` | `margin-bottom: var(--space-3)` | Native tag |
| `section.page-section` | `padding-top`/`padding-bottom: var(--space-7)` | Native structural selector |
| `.card-padding` | `padding: var(--space-4)` | Opt-in class |
| `.card-grid-gap` | `gap: var(--space-5)` | Opt-in class |

**Why cards use opt-in classes, not native selectors:** Squarespace's
project/portfolio grid markup varies a lot by block type — a Summary
Block, Gallery Block, and Portfolio Collection each render completely
different class names. Rather than guess at your site's actual markup,
`.card-padding` and `.card-grid-gap` work like `.text-link`/`.text-cta` —
add the class name in the Squarespace editor's Custom CSS Class field on
whichever card/grid-container elements you want spaced. `.card-grid-gap`
only has a visible effect if the container it's applied to is already
`display: flex` or `display: grid` — worth a quick visual check after
applying.

**Section padding note:** `section.page-section` targets Squarespace
7.1's near-universal section wrapper. If a section already has explicit
padding set in the editor's Section Design panel, that inline setting
always wins over this stylesheet — so existing per-section spacing you've
already dialed in stays intact. If your template doesn't use this exact
class, the rule simply matches nothing (safe no-op); verify via browser
inspector if section padding doesn't visibly change.

**Flagged, not fixed — heading margins vs. this scale:** H4–H6's existing
`margin-bottom` (0.5rem / 8px, from Phase 1) already lands exactly on
`--space-2`. H1/H2 (0.75rem / 12px) and H3 (0.625rem / 10px) fall between
`--space-2` (8px) and `--space-3` (16px) and don't match any step on this
scale. Left as-is per this phase's "don't touch typography tokens" scope —
worth a look if a future pass wants every heading scale-aligned.

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
  Applying colors to backgrounds, borders, and CTAs is a future phase.
- Responsive/breakpoint spacing — fixed values only this pass; deferred to
  after custom CTA/component work.
- Component-level tokens (border-radius, shadow, border-width) — next phase.
- Squarespace's native button component styling (the actual `.sqs-button`
  system) and CTA markup — untouched. `.text-cta` is a standalone text
  style, not a button override.
