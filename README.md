# Typography Token System — suzanne-hunter.com

A foundational, non-destructive typography pass for the Squarespace 7.1 site.
Defines an 8-level type hierarchy as CSS custom properties and applies it via
Design → Custom CSS. No layout, spacing, content, or button-component styling
is touched.

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
  brand color tokens (`--color-primary`, `--color-secondary`) for reuse.
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

## Flag for a future pass

Nunito Sans's proportions (x-height, width) tend to read a touch small at a
16px body size compared to serif-forward body copy. Worth considering a
site-wide bump to 17–18px base (Design → Fonts → Paragraph) later — **not**
changed in this pass, since it's scoped to the token hierarchy, not the
global base size.

## Out of scope (by design)

- Brand colors are declared as tokens but not applied broadly — only used as
  placeholders in `.text-link` / `.text-cta` hover states.
- Section spacing / global padding — untouched.
- Squarespace's native button component styling (the actual `.sqs-button`
  system) — untouched. `.text-cta` is a standalone text style, not a button
  override.
