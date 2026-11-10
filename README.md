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
- **Phase 4 (Page Section template):** a reusable featured-project section
  for the portfolio, with new layout/radius/surface tokens, H1 and H2 one
  step up the scale, a button-hover fix, and three type utility classes. See
  [Page Section template](#page-section-template-phase-4).

No content, block structure, or Squarespace's own button styling is touched.
`typography-tokens.css` is the only source of truth; the two preview files
mirror it by hand. Pasting it into Custom CSS is the last step of any piece
of work, after testing in Code Blocks.

## Install

1. Squarespace admin → **Design → Custom CSS**.
2. Paste the entire contents of [`typography-tokens.css`](typography-tokens.css)
   in (append below anything already there, or replace if this is the whole
   custom stylesheet).
3. Save. Changes apply immediately — no template edits needed.
4. To use the opt-in custom classes (`text-link`, `text-cta`,
   `card-padding`, `card-grid-gap`): write the class name into your own
   markup, for example `<a class="text-link" href="…">` inside a Code Block.
   A class on its own does nothing until some element carries it. (Earlier
   versions of this README said to use a "Custom CSS Class" field on native
   blocks. I haven't been able to confirm that field exists on 7.1 blocks,
   so don't rely on it. If you find one, note its exact label here.)

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
| H1 | `h1` | Ovo (serif) | 3.75rem / 60px | Page / hero title |
| H2 | `h2` | Ovo (serif) | 3rem / 48px | Major section title |
| H3 | `h3` | Nunito Sans | 1.5rem / 24px | Subsection title |
| H4 | `h4` | Ovo (serif) | 1.25rem / 20px | Card / project title |
| H5 | `h5` | Nunito Sans | 0.875rem / 14px | Eyebrow / small subhead (uppercase) |
| H6 | `h6` | Nunito Sans | 0.75rem / 12px | Metadata / caption |
| L7 | `.text-link` | Nunito Sans | 1rem / 16px | Text link |
| L8 | `.text-cta` | Nunito Sans | 0.875rem / 14px | Button/CTA label (uppercase) |

**Why this ratio:** every size is 1rem × 1.25ⁿ, rounded to a clean rem
value. H1 (n=6, 3.815rem → 3.75rem) and H2 (n=5, 3.052rem → 3rem) moved up
one step each in Phase 4, together, so the two headings keep a clear size
difference at every width. At 768px they step down to 48px and 40px, and at
480px to 44px and 36px. H3 (24px) now sits well below H2 (48px) because the
steps in between are unused.

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
| Magenta | 100, 200, 500, 700, 800, 900 |
| Teal | 100, 300, 600, 900 |
| Cool neutral | 50, 100, 300, 600, 900 |
| `--white` | `#FFFFFF` — CTA text on dark backgrounds, and the primary page background |

**Semantic** — what components should actually reference:

| Token | Resolves to | Use |
|---|---|---|
| `--color-background` | white | Primary page background |
| `--color-background-alt1` | cool-50 | Section background variant 1 |
| `--color-background-alt2` | cool-100 | Section background variant 2 |
| `--color-border` | cool-300 | Dividers, outlines |
| `--color-text-body` / `--color-text-muted` | cool-900 / cool-600 | Body copy / secondary text |
| `--color-text-link` / `--color-text-link-hover` | teal-900 / magenta-900 | Inline text links |
| `--color-primary` / `--color-primary-hover` | magenta-900 / magenta-700 | Brand primary |
| `--color-secondary` / `--color-secondary-hover` | teal-900 / teal-600 | Brand secondary |
| `--color-cta-bg` / `--color-cta-bg-hover` / `--color-cta-text` | magenta-900 / magenta-800 / white | Primary CTA/button |
| `--color-cta-secondary-bg` / `-hover` / `-text` | teal-900 / teal-600 / white | Secondary CTA/button |

**Contrast confirmed (no adjustments made to the provided values):**
white text on `--magenta-900` is 8.48:1 (passes AAA); white text on
`--teal-900` is 5.45:1 (passes AA, not AAA); white text on `--magenta-800`
(`#9E429F`, the button hover) is 5.62:1 (passes AA). `--color-primary-hover`
still points at magenta-700 (`#CD18C7`), so it no longer matches the button
hover; left as is, worth aligning later.

**Not yet applied anywhere new.** These tokens don't touch any selector this
pass — that's deliberate, saved for CTA prototyping next. The one real side
effect: `--color-primary`/`--color-secondary` already existed as Phase 1
placeholders (raw hex, "reference only"). `--color-primary` is unchanged
(`#891085`). `--color-secondary` shifts from the old placeholder `#008080`
to the chosen `--teal-900` (`#007580`) — a small shift that `.text-link:hover`
and `.text-cta:hover` (already live from Phase 1) pick up automatically,
since they reference `var(--color-secondary)`.

**Background reworked to white.** `--color-background` now resolves to
`--white` instead of `cool-50`, to save graphics/editing time against
existing white-background assets. The old `cool-50`/`cool-100` background
options aren't gone — renamed to `--color-background-alt1`/`-alt2` for
future section-background variation. Still tokens-only: nothing above is
wired to a live selector yet. Note: the two preview HTML files
(`preview.html`, `preview-squarespace-block.html`) intentionally keep
their own page background on `cool-50` regardless of this change — they're
used as a side-by-side visual reference, and a white preview background
would wash out light swatches (e.g. `cool-50` itself, `magenta-100`).

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
write the class name into the markup of whichever card/grid-container
elements you want spaced (for example inside a Code Block). `.card-grid-gap`
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

## Page Section template (Phase 4)

A reusable featured-project section: a translucent, bordered module with a
project label, title, role, summary, badges, a call-to-action and an image.
It can go anywhere on the site; its first use is directly below the landing
page hero. The approved mockups are in the repo
(`page-section-right_1.png`, `page-section-left_1.png`) and the build spec is
`claude-code-prompt-page-section.md`.

### Constraints that shaped it

| Constraint | Decision |
|---|---|
| Custom CSS can style a page but can't add HTML | The markup lives in a Code Block; the CSS lives once in `typography-tokens.css`. |
| Squarespace's own block settings must still be able to win | Low specificity, no `!important`. Every rule starts with `.sh-section`, so Squarespace's link and heading styles inside a Code Block don't override it. |
| `.page-section` is already Squarespace's section wrapper | The component uses its own prefix, `.sh-section`. |
| The background video is Squarespace's, not mine | The wrapper is transparent; only the module is translucent (white at 75%), so the native section background shows through. |
| One system, not one-off values | Every size, radius, color and gap is a token, with responsive steps at the file's existing 768px and 480px breakpoints. |
| Squarespace's mobile switch is 767px | Buttons stack and go full width at 767px and below. |

### Using it in Squarespace

1. Add a **Code Block** to the page, inside a section.
2. In **Section Design**, set that section's top and bottom padding to **0**.
   The component adds its own (`--section-y`), and the two would stack.
3. Stretch the Code Block across the grid in the editor. The module is capped
   at 72rem, so it never grows past its design width. In testing, a block
   that gave the module about 1130px or more kept the title on one line at a
   1280px window.
4. Paste the markup between the `COPY FROM HERE` and `COPY TO HERE` markers in
   `preview.html`. Until `typography-tokens.css` is in Custom CSS, paste the
   whole `preview-squarespace-block.html` instead, because it carries its own
   tokens.
5. Edit these per use:
   - **Button link:** the `href` on the `sh-button--primary` link (currently
     `[Balancer case study URL]`). It's the only link in the section.
   - **Image:** replace the transparent placeholder `src` with the image URL,
     and update the `alt` text.
   - **Copy:** eyebrow, title, role, summary, badges, caption.
   - **`id` on the title:** it's used by `aria-labelledby`, so keep it unique
     on each page (for example `sh-title-2`).
6. **Image on the left:** add `sh-section--image-left` to the `<section>`
   element's class. It only affects widths above 768px; the DOM order doesn't
   change, so screen readers still read the text first.
7. **A second button:** add it after the primary, using `sh-button--secondary`
   or `sh-button--tertiary`. A commented example is in the markup. Only the
   primary is rendered by default, so there's no empty slot.

### Token changes

| Change | Before | After |
|---|---|---|
| `--font-size-h1` | 3rem / 48px | 3.75rem / 60px (48px at 768px, 44px at 480px) |
| `--font-size-h2` | 2rem / 32px | 3rem / 48px (40px at 768px, 36px at 480px) |
| `--line-height-h1` / `-h2` | 1.1 / 1.15 | 1.05 / 1.1 |
| `--color-cta-bg-hover` | magenta-700 `#CD18C7` | new `--magenta-800` `#9E429F`, matching the live button hover |

**H1 and H2 changed together, and the change is sitewide.** Both moved one
step up the 1.25 scale so the two headings stay clearly different in size.
Every h1 and h2 on the site picks this up once the CSS is live, so check any
page that uses a Heading 1 or Heading 2 text block, and project or blog page
titles. The H1 may be refined when the landing page hero is designed.

**New tokens:** `--measure`, `--content-max`, `--gutter`, `--section-y`,
`--module-pad` (the last three step down at 768px and 480px), the radius set
(`--radius-sm`, `-lg`, `-xl`, `-module-phone`, `-pill`), `--font-size-lead`,
`--font-size-caption`, and `--color-surface-veil`. **New classes:**
`.text-eyebrow` (H5 values as a class, so the project label isn't a heading),
`.text-lead`, `.text-caption`. Headings inside the section use
`--color-text-body`, not Squarespace's heading color.

### Contrast (WCAG 2.2 AA)

Ratios computed with the WCAG formula. The module is white at 75%, so the
color behind it changes the result; the table shows the gradient used as a
stand-in for the video, and pure black as an absolute worst case.

| Pair | Ratio |
|---|---|
| White on `#891085` (primary button) | 8.48:1 |
| White on `#9E429F` (primary button hover) | 5.62:1 |
| Body text over the module, worst case on the gradient (teal-300) | 12.86:1 |
| Magenta eyebrow over the module, worst case on the gradient | 7.55:1 |
| Teal border against the module, worst case on the gradient | 4.85:1 |
| Badge text on white | 14.43:1 |
| Badge outline against the module on the gradient | 3.26 to 3.62:1 (3:1 needed) |
| Over pure black: body text / eyebrow / border | 7.85 / 4.61 / 2.96:1 |

Over any video, text stays above AA even in the worst case. The teal border
and badge outlines are the parts that can dip below 3:1 over very dark
footage, so check them once the real video is in.

### Accessibility checks

- **Keyboard:** the only tab stop is the button; the focus ring is 2px
  teal-900 with a 3px offset. Reading order is text, buttons, then figure,
  in both layouts.
- **Headings:** the section title is an `h2` under the page's `h1`. The
  eyebrow is a paragraph, not a heading.
- **Landmarks and lists:** the section is labelled by its title; the badges
  are a real list (`role="list"` keeps list semantics in Safari).
- **Image:** the `alt` text is in the markup. Update it with each image.
- **Targets and motion:** the button is at least 52px tall, and the
  component has no animation, only 0.15s color transitions.
- **Narrow screens:** no horizontal scroll or clipping at 320px, but the
  button label wraps to two lines there, because the spec's side padding
  leaves little room.

### Background video: what to check in Squarespace

The video isn't in these files; it's Squarespace's own section background.
Before relying on it, check on the live page:

1. **Pause control:** moving content that starts automatically and lasts more
   than 5 seconds needs a way to pause or stop it (WCAG 2.2.2). Look in the
   section's background settings for a playback control. If there isn't one,
   a still image or a separate pause control is needed.
2. **Reduced motion:** in Chrome DevTools, open the Rendering panel and
   emulate `prefers-reduced-motion: reduce`, then see whether the video
   stops. I haven't verified how Squarespace behaves here.
3. **Contrast over real footage:** the 75% module was tested against the
   gradient and against black, not your video. Re-check text, the border and
   the badge outlines over the video's darkest and busiest frames.

### Known limits

- The primary button uses weight 800. Your site loads Nunito Sans at 400,
  700 and 900, so browsers pick the nearest heavier face (900). The live
  site's buttons should render the same way.
- Not yet checked: Safari and Firefox, and Squarespace's own rendering of
  the final Custom CSS (plain `h1`/`h2` rules versus Squarespace's heading
  styles). Verify at paste time.
- Existing issues elsewhere in the file, not changed here:
  `--color-text-muted` (`#728A8D`) is 3.66:1 on white, which fails AA for
  small text, and white on teal-600 (`#00B4C5`) is 2.52:1, which fails.

## Flag for a future pass

Nunito Sans's proportions (x-height, width) tend to read a touch small at a
16px body size compared to serif-forward body copy. Worth considering a
site-wide bump to 17–18px base (Design → Fonts → Paragraph) later — **not**
changed in this pass, since it's scoped to the token hierarchy, not the
global base size.

## Dev tooling (not part of the Squarespace deliverable)

`static-server.mjs` and `preview.html` are local-only files used during Claude
Code work sessions to visually review the tokens in a browser before
anything gets pasted into Squarespace. `preview-squarespace-block.html` is
the same content scoped under `.typo-preview`, so it can be pasted whole into
a Code Block on a test page (e.g. `/testclaude`) without touching the rest
of the site. None of these are meant to be linked from the live site — only
`typography-tokens.css` goes into Design → Custom CSS, and only at the end.

## Out of scope (by design)

- Colors beyond the Page Section: the semantic color tokens are used by the
  Phase 1 link/CTA hover states and the Page Section only. Applying colors
  to other backgrounds, borders, and buttons is a future phase.
- Responsive/breakpoint spacing — fixed values only this pass; deferred to
  after custom CTA/component work.
- Shadow and border-width tokens — not needed yet. Radius tokens arrived
  with the Page Section.
- Squarespace's native button component styling (the actual `.sqs-button`
  system) — untouched. `.text-cta` is a text style and `.sh-button` is
  scoped to the Page Section; neither overrides Squarespace's buttons.
