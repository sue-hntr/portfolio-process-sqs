# Claude Code prompt — Page Section template (draft for Suzanne's review)

Replaces the earlier "case-study hero" prompt. Copy everything below the line into Claude Code, opened in the `portfolio-process-sqs` folder.

---

## Role and goal

You're helping me implement a reusable **Page Section template** for my UX portfolio, suzanne-hunter.com, built on **Squarespace 7.1**. It's a featured-project section that can be used anywhere on the site. Its first use is **directly below the hero on the landing page**. It is not the page hero.

The design is final and was approved on a design canvas. Your job is to translate it faithfully into code that fits my existing design token system. Don't redesign anything.

Work in phases and **stop for my approval at each checkpoint** marked ⏸. Do not commit or push anything; I commit with GitHub Desktop.

## Project files

Everything is in this folder, `portfolio-process-sqs`:

- **`typography-tokens.css`** — my token system for Squarespace's Design → Custom CSS panel (typography Phase 1, color Phase 2, spacing Phase 3). Read it fully before starting. (I may also have a copy named `typography-tokens-copy.css`; if both exist, tell me which is current before editing either.)
- **`preview.html`** and **`preview-squarespace-block.html`** — my existing local preview pages. Read them and tell me what each one is for before you change them.
- **This prompt** — the approved spec (below).

## Constraints (non-negotiable)

- **Squarespace 7.1 Custom CSS panel:** one CSS block, low specificity (single class or tag selectors where possible), so Squarespace's own block-level editor settings can still override. No `!important` unless you show me why it's unavoidable.
- **Extend, don't rewrite.** Add new tokens and rules in clearly labeled sections at the end of the file (e.g. `PHASE 4 — PAGE SECTION TEMPLATE`). Don't change existing token values except the changes listed under "Token changes."
- **Breakpoints:** match my existing ones, `@media (max-width: 768px)` (tablet) and `@media (max-width: 480px)` (phone). The full-width button rule uses `@media (max-width: 767px)` to match Squarespace's mobile switch. (The design canvas used container queries; convert them to these media queries.)
- **Class names:** don't use `.page-section`, which Squarespace already uses for its section wrapper and my spacing rules target. Use a prefixed name such as `.sh-section`, with the modifier `.sh-section--image-left`.
- **Accessibility:** WCAG 2.2 AA minimum. Every color pair below was contrast-checked; don't introduce new colors.
- **Don't invent content.** Use the copy below verbatim. Anything in [brackets] stays a placeholder.

## Phase 1 — Audit and approach ⏸

Before writing CSS:

1. Read `typography-tokens.css` and the two preview files. List which existing tokens the section will use, and summarize what each preview file does.
2. Propose **how the section's markup gets onto a Squarespace 7.1 page**. Custom CSS can't add HTML, so compare:
   - an HTML Code Block containing the section's markup, styled by the CSS;
   - Squarespace's native blocks (text, button, image) styled by targeting the section.
3. **Background video:** I'll add it with Squarespace's own **section background video** setting, so it never goes into my code files. Your CSS must let that native background show through: no opaque backgrounds on the wrapper, only the translucent module. Explain how the module sits over Squarespace's section background in each approach.
4. **Heading sizes:** Squarespace's Design panel also sets heading sizes. Tell me which wins, the panel or my Custom CSS, and whether I need to change anything in the panel for the new H1 and H2 sizes to take effect.
5. Note any Squarespace plan or editor limitations you're unsure of, and tell me how to verify them rather than guessing.
6. Recommend one approach with trade-offs. **Stop and wait for my choice.**

## Phase 2 — Tokens ⏸

Add these tokens to `:root` in a new labeled section. Show me the diff and stop.

```css
/* Layout */
--measure: 40rem;          /* max line length for lead text */
--content-max: 72rem;      /* max width of section content */
--gutter: 4rem;            /* 2rem ≤768px, 1.5rem ≤480px */
--section-y: 6rem;         /* 4rem ≤768px, 3rem ≤480px */
--module-pad: 3rem;        /* 2rem ≤768px, 1.5rem ≤480px */

/* Radius */
--radius-sm: 4px;
--radius-lg: 1rem;         /* image box */
--radius-xl: 2rem;         /* bordered module */
--radius-module-phone: 1rem; /* module corners ≤480px; set to var(--radius-xl) to keep 32px */
--radius-pill: 999px;      /* buttons, badges */

/* Type */
--font-size-lead: 1.125rem;    /* line-height 1.6 */
--font-size-caption: 0.875rem; /* line-height 1.5 */

/* Surfaces */
--color-surface-veil: rgba(255, 255, 255, 0.75); /* translucent module */
```

Responsive values go in the existing `:root` media-query overrides, the same way my heading sizes already step down.

**Token changes:**

1. **H1 and H2 sizes, changed together.** Both move one step up my existing 1.25 ("Major Third") scale, so headings keep a clear size difference everywhere on the site:

   | Token | Desktop | ≤768px | ≤480px | Line-height |
   | --- | --- | --- | --- | --- |
   | `--font-size-h1` | 3.75rem (60px) | 3rem (48px) | 2.75rem (44px) | 1.05 |
   | `--font-size-h2` | 3rem (48px) | 2.5rem (40px) | 2.25rem (36px) | 1.1 |

   - H1 is 1rem × 1.25⁶ = 3.815rem, rounded to 3.75rem. H2 is 1.25⁵ = 3.052rem, rounded to 3rem. That's the same rounding method the file's scale notes already document, so update those notes and the LEVEL MAP comment to match.
   - Update the `--line-height-h1` and `--line-height-h2` tokens to the values above.
   - **This affects every h1 and h2 sitewide.** List which pages or blocks use them so I can check them. I may refine the H1 size later when I design the landing page hero.
2. **Button hover.** `--color-cta-bg-hover` points to magenta-700 (#CD18C7), but my live Squarespace buttons use **#9E429F**. Add a primitive (e.g. `--magenta-800: #9E429F`) and point `--color-cta-bg-hover` at it. White on #9E429F is 5.62:1. Flag anything else that references the old value.

**New utility classes:**
- `.text-eyebrow` — same values as H5 (0.875rem, weight 600, letter-spacing 0.08em, uppercase), as a class so the project label isn't a heading above the h2.
- `.text-lead` — `--font-size-lead`, line-height 1.6, max-width `--measure`.
- `.text-caption` — `--font-size-caption`, weight 400, line-height 1.5, color `--color-text-body`.

## Phase 3 — Page Section component ⏸

Build one component with a **default layout (image right)** and a **modifier for image left on desktop** (`.sh-section--image-left`).

### Structure and copy (in this DOM order)

1. **Section wrapper.** Transparent, so Squarespace's native section background (video or color) shows through.
2. **Bordered module** (inset inside the page gutters), containing:
   - Eyebrow `.text-eyebrow`: **Balancer** (magenta-900, `--color-primary`)
   - `<h2>`: **Build Trust in Healthcare AI** (Ovo, H2 token)
   - Role meta line: small label **Role** (H6 style: 0.75rem, weight 500, letter-spacing 0.04em, uppercase) above value **UX Design/Design Leadership** (1rem, weight 600)
   - Lead paragraph `.text-lead`: **My work on Balancer brings systems thinking, accessibility, documentation, and safe interaction design into a healthcare product where clarity and trust matter. Over time, that work expanded from individual contribution into design leadership.**
   - Badges as a `<ul>`: **Healthcare AI · Open source (Code for Philly) · Design system · WCAG accessibility**
   - Button group: primary button **See work & outcomes**, linking to the Balancer case study page: `href="[Balancer case study URL]"`. The link must be easy to change each time the template is reused.
   - `<figure>` with the image and `<figcaption class="text-caption">`: **The product was originally built by engineers from Tailwind specs; I began translating it into a workable design system in Figma.**
   - Image alt text: **"Balancer website app main screen and Balancer design system screen in Figma"**

### Visual spec

**Module:**
- `max-width: var(--content-max)`, centered. The section has `--section-y` padding top and bottom and `--gutter` left and right.
- Border: **2px solid teal-900** (`--color-secondary`, #007580).
- Corners: `--radius-xl` (32px) on desktop and tablet; `--radius-module-phone` (16px) at ≤480px.
- Background: `--color-surface-veil` (white at 75%).
- Padding: `--module-pad`.

**Desktop grid (above 768px):** 12 columns, column gap 1.5rem.
- Default (image right): text block (eyebrow through badges) in columns 1–7, button group under it in columns 1–7, figure in columns 8–12, stretching to the text height (image min-height 360px).
- `.sh-section--image-left`: figure in columns 1–5, text and buttons in columns 6–12. **Keep the DOM order unchanged** (text before figure) so screen readers read the text first.

**Tablet and phone (≤768px):** single column: text block → button group → figure. The figure sits 2rem below the buttons and the image becomes 16:9 (`aspect-ratio: 16 / 9`). The image-left modifier has no effect at these widths.

**Badges:** white fill, 1px solid cool-600 (#728A8D) outline, cool-900 text (14.4:1), 0.875rem, weight 600, padding 6px 14px, `--radius-pill`, 8px gap, wrapping. 1rem below the lead paragraph, 2rem above the button group.

**Image:** placeholder fill cool-100 (#EAF3F4) until the real image is added; `--radius-lg` corners; caption 8px below.

**Buttons** (rebuilt from my live Squarespace buttons):
- Base: Nunito Sans 1rem, sentence case, min-height 52px, padding 0.875rem 2.5rem, min-width 220px.
- **Primary:** magenta-900 #891085 fill, white text (8.48:1), weight 800, `--radius-pill`. Hover: #9E429F (5.62:1).
- **Secondary:** white fill, 1px magenta-900 outline, magenta text, weight 700, pill. Hover: fills magenta-900, white text.
- **Tertiary:** transparent, magenta text, 2px magenta-900 bottom rule, square corners, weight 700. Hover: square magenta-900 block, white text.
- **Focus:** visible `:focus-visible` ring, 2px solid teal-900, offset 3px.
- Transition color, background and border over 0.15s.
- **Button group holds 1 or 2 buttons.** A second action (secondary or tertiary) goes after the primary: side by side with a 1rem gap above 767px; stacked full width at ≤767px, primary first, 0.75rem gap. Only the primary is used now, so don't render an empty slot.
- My sitewide Squarespace buttons already use these styles. Prefer reusing or matching Squarespace's native button classes over duplicating them, and tell me which you chose.

## Phase 4 — Local preview, verify and document ⏸

1. **Preview:** add the section to the appropriate existing preview file (or both, if that's how they're meant to work). Show both layouts, image right and image left, under a placeholder hero whose h1 uses the new H1 size, so I can see the H1/H2 relationship. Stand in for the Squarespace background video with this gradient, in the preview only, never in `typography-tokens.css`:
   `linear-gradient(115deg, #F7FBFC 0%, #D4F4F7 45%, #4CE8F7 100%)` (cool-50 → teal-100 → teal-300).
2. **Contrast:** check every color pair and list the ratios. Expected:

   | Pair | Ratio |
   | --- | --- |
   | Body text over the 75% module (worst case, over teal-300) | ~12.9:1 |
   | Magenta eyebrow over the module | ~7.6:1 |
   | Teal border against the module | ~4.9:1 |
   | White on #891085 | 8.48:1 |
   | White on #9E429F | 5.62:1 |

3. **Accessibility checks:** keyboard order, focus visibility, heading order (one h1 on the page in the hero; this section's title is h2; the eyebrow is not a heading), and alt text.
4. **Widths:** test at 1280, 768 and 390px. Confirm the 44px H1 and 36px H2 wrap cleanly at 390px.
5. **Background video:** don't build it, but tell me what I need to check in Squarespace's native section video. Can visitors pause it (required for motion longer than 5 seconds, WCAG 2.2.2)? Does it respect reduced-motion settings? Once the real video is in, I'll need to re-check text contrast over its darkest and busiest frames, because the 75% module was tested only against the gradient.
6. **README:** write a short section covering what was added, how to use the template and its image-left option in Squarespace, how to change the button link per use, and the token changes, including the H1/H2 scale step. Write it so it can double as portfolio documentation, with a constraint-driven, systems-thinking angle.

## Known issues elsewhere in the file (don't fix now, just note)

- `--color-text-muted` (#728A8D) fails AA for small text: 3.66:1 on white.
- White on teal-600 (#00B4C5, `--color-secondary-hover` / `--color-cta-secondary-bg-hover`) fails at 2.52:1.

## How to work with me

- Ask before assuming anything about Squarespace you can't verify.
- Show diffs, not whole files, when you change `typography-tokens.css`.
- Keep changes small and phased so they don't disrupt existing content.
