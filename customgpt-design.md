---
name: customgpt-design
description: Use this skill to generate well-branded interfaces, mockups, and assets for CustomGPT.ai. Strict design system execution mode — all output must use defined tokens only. Includes brand tokens (colors, type, spacing, radius, shadows), Public Sans + Inter fonts, Vuetify SCSS configuration (vuetify.scss), production component wrappers, two reference UI kits (chat + dashboard), and ~20 component preview cards. Invoke when designing anything that should feel like CustomGPT — chat surfaces, admin/studio screens, marketing slides, widgets, or component spec sheets.
user-invocable: true
---

# CustomGPT.ai Design System — AI Execution Skill (Strict Mode)

Design system root: `/Users/miodragristovski/Downloads/CustomGPT.ai Design System-2/`

---

## CORE RULE (NON-NEGOTIABLE)

The design system is the single source of truth.

- Do not introduce new tokens
- Do not modify existing tokens
- Do not use raw values (px, hex, arbitrary numbers) — use semantic tokens
- Do not invent new components or variants

If something is not defined → use the closest existing semantic token. Do NOT create a new one.

You are not designing. You are executing a predefined design system.

---

## TOKEN HIERARCHY

Raw → Semantic → Component → Usage

- NEVER use raw tokens directly
- ALWAYS use semantic or component tokens
- ALWAYS prefer component tokens over semantic when available

---

## COLOR TOKENS (CustomGPT values)

### Brand
| Token | Value |
| --- | --- |
| `brand-primary-default` | `#7367F0` |
| `brand-primary-hover` | `#685DD8` |
| `brand-primary-active` | `#5C53C0` |
| `brand-primary-tint` | `#EAE8FD` (chip / soft fill) |

### Background / Surface
| Token | Value |
| --- | --- |
| `bg-canvas` | `#FAFAFA` |
| `bg-surface` | `#FFFFFF` |
| `bg-elevated` | `#FFFFFF` + shadow |
| `bg-selected` | `#F5F5F5` |
| `bg-overlay` | `rgba(23,23,23,0.5)` |

### Typography Colors
| Token | Value | Notes |
| --- | --- | --- |
| `text-heading` | `#212121` | production `heading` key |
| `text-body` | `#565656` | production `body` key |
| `text-muted` | `#B7B5BE` | production `muted` key |
| `text-disabled` | `#A3A3A3` | |
| `text-placeholder` | `#999999` | production `placeholder` key |
| `text-link` | `#7367F0` | |

### Borders / Dividers
| Token | Value |
| --- | --- |
| `border-default` | `#E5E5E5` |
| `border-emphasis` | `#D4D4D4` |

### Status
| Token | Value | Notes |
| --- | --- | --- |
| `color-success` | `#28C76F` | |
| `color-warning` | `#FF9F43` | |
| `color-error` / `color-danger` | `#EA5455` | production key is `danger` in Vuetify/Tailwind |
| `color-info` | `#0076E5` | blue — NOT teal |

### Disabled
| Token | Value |
| --- | --- |
| `bg-disabled` | `#DBDADE` |
| `text-disabled` | `#A3A3A3` |

---

## HDR / DISPLAY P3 — PROGRESSIVE ENHANCEMENT

CustomGPT.ai targets HDR-capable displays (Apple XDR, P3-1600 nits). HDR is a CSS-level enhancement — token names stay the same; P3 values are richer expressions of the same tokens. No new tokens are introduced.

### Rule

- sRGB hex values in the token table are the **fallback** (all displays)
- `@media (color-gamut: p3)` block upgrades vivid tokens on capable displays
- Neutral tokens (backgrounds, text, borders) get no P3 override — delta is imperceptible
- Design P3-first (how it looks on an XDR display), then derive the sRGB fallback — never the reverse

### Which tokens get P3 overrides

| Token | sRGB fallback | Display P3 value |
| --- | --- | --- |
| `brand-primary-default` | `#7367F0` | `color(display-p3 0.45 0.40 0.94)` |
| `brand-primary-hover` | `#685DD8` | `color(display-p3 0.41 0.36 0.85)` |
| `brand-primary-active` | `#5C53C0` | `color(display-p3 0.36 0.32 0.75)` |
| `color-success` | `#28C76F` | `color(display-p3 0.15 0.78 0.43)` |
| `color-warning` | `#FF9F43` | `color(display-p3 1.00 0.62 0.26)` |
| `color-error` | `#EA5455` | `color(display-p3 0.92 0.33 0.33)` |
| `color-info` | `#0076E5` | `color(display-p3 0.00 0.46 0.90)` |

### Tokens that do NOT get P3 overrides

Backgrounds, text, borders, and disabled colors — near-neutral values that are visually identical across color spaces. Do not add P3 overrides for these.

### CSS block to add to `colors_and_type.css`

```css
@media (color-gamut: p3) {
  :root {
    --brand-primary-default: color(display-p3 0.45 0.40 0.94);
    --brand-primary-hover:   color(display-p3 0.41 0.36 0.85);
    --brand-primary-active:  color(display-p3 0.36 0.32 0.75);

    --color-success: color(display-p3 0.15 0.78 0.43);
    --color-warning: color(display-p3 1.00 0.62 0.26);
    --color-error:   color(display-p3 0.92 0.33 0.33);
    --color-info:    color(display-p3 0.00 0.46 0.90);
  }
}
```

### HTML `<head>` baseline (every page)

```html
<meta name="color-scheme" content="light dark">
```

### Biggest visual win

`brand-primary-default` — buttons, links, active states, and `shadow-cta` all reference this token. The P3 purple is noticeably richer than sRGB on capable displays and requires zero component changes.

---

## SPACING TOKENS

### Semantic spacing (ONLY allowed)
| Token | Value |
| --- | --- |
| `spacing-xs` | 4px |
| `spacing-sm` | 8px |
| `spacing-md` | 12px |
| `spacing-lg` | 16px |
| `spacing-xl` | 24px |
| `spacing-2xl` | 32px |
| `spacing-3xl` | 48px |

### Layout / Section spacing
| Token | Value |
| --- | --- |
| `section-sm` | 48px |
| `section-md` | 64px |
| `section-lg` | 96px |
| `section-xl` | 128px |

Rules:
- ALWAYS use semantic spacing tokens
- NEVER use arbitrary spacing (e.g. 18px, 10px, 6px)
- 4-point grid only — valid multiples: 4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 48, 64, 80, 128
- Use layout tokens for sections; use semantic tokens for component internals
- Maintain consistent vertical rhythm

---

## RADIUS TOKENS

| Token | Value | Use |
| --- | --- | --- |
| `radius-none` | 0px | — |
| `radius-sm` | 4px | small controls |
| `radius-md` | 8px | buttons, inputs |
| `radius-lg` | 12px | chat bubbles |
| `radius-xl` | 16px | cards, composer |
| `radius-xl` | 16px | cards, composer, modals |
| `radius-full` | 999px | pills, badges, avatars |

Rules:
- Buttons → `radius-md`
- Cards → `radius-xl`
- Pills / badges → `radius-full`
- Inputs → `radius-md`
- **Modals → `radius-xl` (16px) — always use `border-radius: var(--radius-xl)`**
- Do not mix radius styles randomly within the same surface

---

## TYPOGRAPHY TOKENS

### Font family
- **Vuetify components** → `Public Sans` (set via `$body-font-family` in `vuetify.scss`)
- **Tailwind `font-sans`** → `Inter` variable (set in `tailwind.config.js`)
- **Mono** → `JetBrains Mono` / `ui-monospace` — tokens, kbd, API keys only

Both fonts coexist. Vuetify renders its own components (VCard, VBtn, VTextField…) in Public Sans. Tailwind utility class `font-sans` applies Inter. Do not force Inter onto Vuetify components — let the SCSS setting stand.

### Size scale (Tailwind v3 standard)
| Class | rem | px |
| --- | --- | --- |
| `text-xs` | 0.75rem | 12px |
| `text-sm` | 0.875rem | 14px |
| `text-base` | 1rem | 16px |
| `text-lg` | 1.125rem | 18px |
| `text-xl` | 1.25rem | 20px |
| `text-2xl` | 1.5rem | 24px |
| `text-3xl` | 1.875rem | 30px |
| `text-4xl` | 2.25rem | 36px |

Headings use custom px values (defined in `addBase`): h1=38px · h2=32px · h3=26px · h4=22px · h5=18px · h6=14px.
Body text (`p`) uses `text-base` (16px). Use Tailwind utility classes directly — do not hardcode px values.

### Line height
| Token | Value |
| --- | --- |
| `leading-tight` | 1.2 |
| `leading-normal` | 1.4 |
| `leading-relaxed` | 1.6 |
| `leading-loose` | 1.8 |

### Font weight
| Token | Value |
| --- | --- |
| `weight-regular` | 400 |
| `weight-medium` | 500 |
| `weight-semibold` | 600 |
| `weight-bold` | 700 |

### Usage rules
- Body text → `text-base` (16px) + `leading-normal` + `weight-regular`
- Small / secondary text → `text-sm` (14px)
- Captions / helpers → `text-xs` (12px)
- Label → `text-sm` + `weight-medium`
- Headings → use h1–h6 elements (sizes defined in `addBase`) or explicit Tailwind size classes
- Avoid more than 3 font sizes per screen
- Never hardcode pixel values for font sizes — use Tailwind utility classes

---

## BORDER TOKENS

| Token | Value |
| --- | --- |
| `border-none` | 0px |
| `border-thin` | 1px |
| `border-thick` | 2px |

Rules:
- Default → `border-thin` with `border-default` (#E5E5E5)
- Emphasis → `border-thick`
- Avoid unnecessary borders

---

## SHADOW TOKENS

| Token | Value |
| --- | --- |
| `shadow-sm` | `0 2px 4px rgba(23,23,23,.08)` |
| `shadow-default` | `0 4px 18px rgba(23,23,23,.08)` |
| `shadow-card` | `0 4px 24px rgba(23,23,23,.06)` |
| `shadow-cta` | `0 4px 24px rgba(115,103,240,.35)` (purple CTAs only) |

---

## COMPONENT TOKENS

### Button

**Variants:**

| Variant | Background | Text | Border | Hover bg |
| --- | --- | --- | --- | --- |
| `btn-primary` | `brand-primary-default` | `#fff` | transparent | `brand-primary-hover` |
| `btn-secondary` | `bg-surface` | `brand-primary-default` | `brand-primary-default` | `brand-primary-tint` |
| `btn-ghost` | transparent | `text-body` | `border-default` | `bg-selected` |
| `btn-destructive` | `color-error` (#EA5455) | `#fff` | transparent | `#D44849` |
| `btn-destructive-ghost` | transparent | `color-error` | `color-error` | `#FEF2F2` |

**Sizes:**

| Class | Height | H-padding | Font |
| --- | --- | --- | --- |
| `.btn-xs` | 24px | `spacing-sm` (8px) | `text-xs` (12px) |
| `.btn-sm` | 32px | `spacing-md` (12px) | `text-xs` (12px) |
| `.btn` (default / md) | 40px | `spacing-lg` (16px) | `text-sm` (14px) |
| `.btn-lg` | 48px | `spacing-xl` (24px) | `text-md` (16px) |

Rules:
- All sizes use `height` property (not vertical padding) — heights are exact
- Radius → `radius-md` for all sizes including lg
- Disabled = `bg-disabled` fill, `text-disabled` text, `pointer-events: none` — never a tinted primary
- Dropdown trigger buttons: `justify-content: space-between` so label pins left, chevron pins right
- Define hover, active, disabled states for every variant

### Input
| Token | Value |
| --- | --- |
| `input-bg` | `bg-surface` |
| `input-border-default` | `border-default` |
| `input-border-hover` | `border-emphasis` |
| `input-border-focus` | `brand-primary-default` |
| `input-border-error` | `color-error` |

Rules:
- Label ALWAYS visible above input
- Error message displayed below input using `color-error`
- Placeholder uses `text-placeholder`
- Radius → `radius-md`

### Card
| Token | Value |
| --- | --- |
| `card-default-bg` | `bg-surface` |
| `card-subtle-bg` | `bg-canvas` |
| `card-elevated-bg` | `bg-surface` + `shadow-card` |

Rules:
- Padding → `spacing-xl` (24px)
- Radius → `radius-xl` (16px)
- Border → `border-thin` with `border-default`
- Use surface tokens only; no raw fills

### Icons
| Token | Use |
| --- | --- |
| `icon-primary` | `text-heading` |
| `icon-secondary` | `text-muted` |
| `icon-disabled` | `text-disabled` |
| `icon-success` | `color-success` |
| `icon-error` | `color-error` |
| `icon-warning` | `color-warning` |
| `icon-info` | `color-info` |

Rules:
- **Always use `<Icon>` — never `<VIcon>` or `@iconify/vue` directly in feature code**
- `Icon.vue` wraps `VIcon` and constructs the icon string as `` `${family}:${icon}` ``
- Default family: `tabler` — omit the `family` prop unless using a different collection
- Icons are served via **Iconify API** (`https://api.iconify.design`) — loaded on demand, no webfont needed
- Vuetify's icon set is registered as `iconify` using `@iconify/vue`'s `Icon` component as the renderer

```vue
<!-- Correct usage -->
<Icon icon="x" />
<Icon icon="chevron-down" size="16" />
<Icon icon="robot" size="20" color="#7367F0" />
<Icon icon="check" family="mdi" />   <!-- only when non-tabler set needed -->

<!-- Wrong — never use these in feature code -->
<VIcon icon="tabler:x" />
<i class="ti ti-x" />
```

---

## LAYOUT RULES

- Default layout → vertical stack
- Use semantic spacing tokens for gaps (`spacing-*`)
- Use layout tokens for sections (`section-*`)
- Align left by default
- Avoid center alignment except empty states and chat surface
- No `backdrop-filter: blur` — solid surfaces only
- No neon gradients, no full-bleed photography

### Surface contexts

**Dashboard (admin/studio):**
- 260px fixed sidebar (collapsible to 72px)
- 24px content gutter (`spacing-xl`)
- Full-width on large screens
- Cards: `bg-surface; radius-xl; border-thin border-default; shadow-card; padding spacing-xl`

**Chat surface:**
- Single-column, center-aligned, max 760px
- Input anchored to bottom
- Chat bubbles: `radius-lg` (12px)

---

## INTERACTION RULES

Always define all three states:
- Hover state
- Active state
- Disabled state

Always provide feedback:
- Success state
- Error state
- Loading state

Transitions:
- State changes (color, border): 120ms
- Entry / exit: 200–280ms
- Easing: `cubic-bezier(.2,.7,.3,1)`

Tooltips:
- Fade in after 133ms hover delay with 4px upward slide
- Dark `text-heading` (#171717) pill, `radius-sm`

---

## COPYWRITING RULES

- Sentence case in UI copy ("Create agent", not "Create Agent")
- Title case only for plan/product names (`Pro`, `Enterprise`)
- You-forward: "Build your agent", not "We'll build..."
- Action verbs, present tense: "Create project", "Upload files"
- No hype adjectives (revolutionary, cutting-edge) — features speak
- "Knowledge base" and "sources" in UI — not "RAG" or "embeddings"
- **No uppercase labels** — use regular sentence-case text for section labels, group headers, and category names. Never use `text-transform: uppercase` or the `uppercase` Tailwind class for UI labels.

---

## DECISION FRAMEWORK

When generating UI:

1. Use existing components
2. Use semantic tokens
3. Prefer simplest solution
4. Avoid new patterns
5. Use spacing for hierarchy, not colors

---

## ANTI-PATTERNS (FORBIDDEN)

- Using raw hex or px values instead of tokens
- Hardcoding arbitrary spacing (e.g. 18px, 10px, 6px)
- Creating new colors outside the defined scale
- Mixing multiple radius styles randomly on the same surface
- Over-designing UI
- Ignoring the component system
- `backdrop-filter: blur`
- Emoji in UI
- Additional typefaces beyond Inter and JetBrains Mono
- Disabled state using a tinted primary color
- Adding P3 overrides to neutral tokens (backgrounds, text, borders) — delta is imperceptible, adds noise
- Designing colors in sRGB first and expecting P3 to match — always design P3-first on an XDR display

---

## OUTPUT FORMAT

Every UI output MUST include:

- Components used (by name)
- Tokens applied — color, spacing, typography
- Layout structure
- Interaction states defined (hover, active, disabled)

---

## FALLBACK RULE

If something is unclear:

- Use the closest existing token
- Use the simplest component
- Do NOT invent a new solution

---

## FILE REFERENCES

| Asset | Path |
| --- | --- |
| CSS tokens (colors, type, spacing, radius, motion) | `<root>/colors_and_type.css` |
| Logo mark (purple dual-G) | `<root>/assets/logo-48px.svg` |
| Logo wordmark | `<root>/assets/logo-wordmark.svg` |
| Dashboard UI kit | `<root>/ui_kits/dashboard/index.html` |
| Chat UI kit | `<root>/ui_kits/chat/index.html` |
| Preview cards | `<root>/preview/*.html` |
| Inter variable font | `<root>/fonts/Inter-VariableFont_opsz_wght.ttf` |

Link tokens CSS in any new HTML output:
```html
<link rel="stylesheet" href="/Users/miodragristovski/Downloads/CustomGPT.ai Design System-2/colors_and_type.css">
```

## Component inventory (preview cards)

**Design system preview cards** (`<root>/preview/*.html`):
`brand-mark` · `colors-primary` · `colors-gray` · `colors-semantic` · `colors-opacity` · `colors-gradients` · `type-headings` · `type-body` · `spacing-scale` · `radius-scale` · `shadows` · `iconography` · `components-buttons` · `components-inputs` · `components-badges` · `components-avatars` · `components-cards` · `components-alerts` · `components-chat` · `components-composer`

**Extended library** (`/Users/miodragristovski/Desktop/settings-panel.html`):
`button` (5 variants × 4 sizes + disabled matrix) · `input` · `badge` · `avatar` · `alert` · `checkbox` · `toggle` · `select` · `tabs` · `segmented-control` · `tooltip` · `project-card` (with deploy dropdown) · `paginator` (with all states + rules) · `citation-panel` (collapsed / internal / external) · `plan-card` (free / premium / enterprise states) · `change-password-card` · `two-factor-auth-card` · `confirm-password-modal`

## How to use this skill

1. **Read `colors_and_type.css`** — every CSS variable and semantic `.cg-*` class lives there. Always link it.
2. **Browse `preview/*.html`** — copy patterns from these rather than inventing new ones.
3. **Start from a UI kit when relevant** — `ui_kits/dashboard/` for admin/studio, `ui_kits/chat/` for the chat surface.
4. **When invoked without specific guidance** — ask what surface, audience, fidelity, and variations are needed, then output static HTML (linking the CSS) or production-ready code in their stack.

---

## EDITORIAL SURFACE — BLOG POSTS & LONG-FORM DOCUMENTS

Applies to: blog posts, release notes, white papers, documentation articles, any full-reading surface.

Style reference: science journals (Nature, Scientific American) and art books — high white space, measured rhythm, images earn their space.

---

### GOVERNING CONSTANTS

| Constant | Symbol | Value |
| --- | --- | --- |
| Pi | π | 3.14159… |
| Golden ratio | φ | 1.61803… |
| Editorial base unit | `eu` | 16px (body font size) |

---

### π WHITE SPACE SCALE

White space in editorial surfaces is not arbitrary — it is derived from π applied to the editorial base unit (16px).

| Token | Formula | Computed | Nearest semantic token |
| --- | --- | --- | --- |
| `editorial-gap-inline` | `eu × π⁰` | 16px | `spacing-lg` |
| `editorial-gap-paragraph` | `eu × π^0.5` | ~28px | `spacing-2xl` (32px) |
| `editorial-gap-section` | `eu × π¹` | ~50px | `spacing-3xl` (48px) |
| `editorial-gap-chapter` | `eu × π²` | ~158px | `section-lg` (96px) / `section-xl` (128px) |
| `editorial-margin-column` | `eu × π¹` | ~50px | side breathing room on text column |

CSS declaration (add to `:root` on editorial surfaces):
```css
:root {
  --editorial-eu:              16px;
  --editorial-gap-inline:      16px;            /* eu × π⁰  */
  --editorial-gap-paragraph:   32px;            /* eu × π^0.5 → nearest 4pt: 32px */
  --editorial-gap-section:     48px;            /* eu × π¹  → nearest 4pt: 48px  */
  --editorial-gap-chapter:     128px;           /* eu × π²  → nearest 4pt: 128px */
}
```

Rules:
- Between paragraphs (non-indented style) → `editorial-gap-paragraph`
- Between article sections (H2 boundary) → `editorial-gap-section` above heading, half below
- Between major chapters or page-level breaks → `editorial-gap-chapter`
- Margin above pull quotes, figures, or callouts → `editorial-gap-section`
- Side column margin on text column → `editorial-gap-section` (50px approximated to 48px)
- NEVER use arbitrary spacing on editorial surfaces — use only the π-derived tokens above

---

### φ GOLDEN RATIO — IMAGE TO TEXT

The golden ratio governs the proportional split between text and images/figures on any editorial surface.

| Role | Ratio | Value |
| --- | --- | --- |
| Text column (major) | φ minor = `1/φ` | **61.8%** of available width |
| Image / sidebar (minor) | `1 - 1/φ` | **38.2%** of available width |
| Breakout image (1.5× text) | `1/φ × 1.5` | ~92.7% → full bleed with gutter |

**Text column max-width:** 680px (optimal ~65-character line at 16px Inter).

**Two-column editorial layout (text + figure side by side):**
```css
.editorial-row {
  display: grid;
  grid-template-columns: 61.8fr 38.2fr;   /* φ split */
  gap: var(--editorial-gap-section);      /* π-derived gap */
  align-items: start;
}
.editorial-row.image-lead {
  grid-template-columns: 38.2fr 61.8fr;  /* image first, then text */
}
```

**Full-width article with centered text column:**
```css
.editorial-body {
  max-width: 680px;
  margin-inline: auto;
  padding-inline: var(--editorial-gap-section);  /* π margin */
}
```

**Pull quote (occupies minor column = 38.2%):**
```css
.pull-quote {
  width: 38.2%;
  float: right;
  margin-left: var(--editorial-gap-section);
  margin-bottom: var(--editorial-gap-paragraph);
}
```

**Figures / images within articles:**
- Inline (within text column) → 100% of text column width
- Side figure → 38.2% of page width (φ minor)
- Breakout / hero → full container width; caption below in φ-minor width (61.8%)
- Image aspect ratios → prefer 16:9, 3:2, or 1:1 (do not crop to arbitrary ratios)

Rules:
- Every image MUST have a caption in `text-sm` + `text-muted` + `leading-relaxed`
- Image-to-text ratio on any given screen ≤ φ (images never dominate over text)
- White space around figures = `editorial-gap-section` (π-derived) on all sides

---

### EDITORIAL TYPOGRAPHY

| Role | Size token | Weight | Leading | Color |
| --- | --- | --- | --- | --- |
| Article title | `text-3xl` (32px) | `weight-bold` | `leading-tight` (1.2) | `text-heading` |
| Subtitle / deck | `text-xl` (20px) | `weight-regular` | `leading-relaxed` (1.6) | `text-muted` |
| Section heading (H2) | `text-2xl` (24px) | `weight-semibold` | `leading-tight` | `text-heading` |
| Sub-heading (H3) | `text-lg` (18px) | `weight-semibold` | `leading-normal` | `text-heading` |
| Body / running text | `text-md` (16px) | `weight-regular` | `leading-relaxed` (1.6) | `text-body` |
| Pull quote | `text-xl` (20px) | `weight-medium` | `leading-relaxed` | `brand-primary-default` |
| Figure caption | `text-sm` (14px) | `weight-regular` | `leading-relaxed` | `text-muted` |
| Byline / metadata | `text-sm` (14px) | `weight-medium` | `leading-normal` | `text-muted` |
| Footnote / endnote | `text-xs` (12px) | `weight-regular` | `leading-relaxed` | `text-muted` |

Rules:
- Body uses `leading-relaxed` (1.6) — not `leading-normal` — for reading comfort
- Article title tracking: `letter-spacing: -0.02em` (tight, editorial feel)
- Section headings: `letter-spacing: -0.01em`
- Pull quote: left border `4px solid brand-primary-default`, padding-left `spacing-xl`
- Byline sits `editorial-gap-inline` below deck, separated by `border-thin border-default`
- Never more than 4 type sizes on a single editorial page

---

### ILLUSTRATION LIBRARY

All editorial illustrations live at: `/Users/miodragristovski/Documents/Yettel/CustomGPT/`

**Visual DNA (shared across all flat editorial illustrations):**
- Style: flat vector, 2D
- Palette: golden yellow + dark navy + warm orange/red + soft peach/cream backgrounds
- Motifs: human figures in professional settings, document/paper props, organic plant accents
- Background: white or near-white — never dark

**Catalogue:**

| Name | File | Use | Aspect |
| --- | --- | --- | --- |
| `landing-page-hero` | `landing page hero image.png` | Article hero, landing page top | 16:9 |
| `city-documents` | `miodragristovski_create_a_illustration_for_CustomGPT.ai_in_a_st_5541a207-0099-44d2-8cc7-d2947255e79b.png` | φ side figure, section concept | crop 3:2 |
| `team-analytics` | `miodragristovski_make_and_illustration_in_refferenced_style_t_27d9f2c0-edea-4ac5-a53f-6976b8fc3baf_0.png` | φ side figure, data/insights | crop 3:2 |
| `blog-hero-landscape` | `Pick your AI model/Blog Hero image.png` | Blog hero, announcement banners | 16:9 |
| `model-layers-diagram` | `Model Layers/CustomGPT layers.png` | Architecture explainers, technical docs | Full-width |

**Style families — never mix on the same surface:**
- **Flat editorial** (`landing-page-hero`, `city-documents`, `team-analytics`) — warm palette, people + documents
- **Cinematic landscape** (`blog-hero-landscape`) — cool-toned, minimal, brand mark
- **Isometric diagram** (`model-layers-diagram`) — technical explainer only

**Illustration consistency rules:**
- `border-radius: var(--radius-xl)` on every image container — no exceptions
- No `box-shadow` on illustrations — they carry their own soft edges
- Caption always below: `text-sm` + `text-muted` + `font-style: italic`
- Gap above and below any figure: `editorial-gap-section` (48px)
- Illustrations only on `bg-surface` (#FFFFFF) — never on coloured or dark backgrounds
- `alt` text always populated — describe scene content, not filename
- Hero images: `object-fit: cover`, container `aspect-ratio: 16/9`
- Side figures: `object-fit: cover`, container `aspect-ratio: 3/2`
- Diagram images: `object-fit: contain` or full-width at natural dimensions
- Image-to-text ratio on any given screen ≤ φ (images never dominate over text in aggregate)

---

### EDITORIAL SURFACE LAYOUT PATTERN

```
┌─────────────────────────────────────────────────────┐
│           [editorial-gap-chapter top margin]        │
│  ┌────────────────── 680px max ──────────────────┐  │
│  │  ARTICLE TITLE                                │  │
│  │  deck subtitle                                │  │
│  │  [gap-inline] byline · date · read time       │  │
│  │  ───────────────────────────────────────────  │  │
│  │  [gap-section]                                │  │
│  │  Body paragraph...                            │  │
│  │  [gap-paragraph]                              │  │
│  │  Body paragraph...                            │  │
│  │  [gap-section]                                │  │
│  │  ## Section heading                           │  │
│  │  [gap-paragraph / 2]                          │  │
│  │  Body paragraph...                            │  │
│  │                           [38.2%] ┌─────────┐ │  │
│  │  Body continues  ←φ text→         │ FIGURE  │ │  │
│  │  wrapping beside              61.8%└─────────┘ │  │
│  │  the image column.            caption text     │  │
│  │  [gap-chapter]                                │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

---

## EXTENDED COMPONENT LIBRARY

Live reference: `/Users/miodragristovski/Desktop/settings-panel.html`

---

### Plan Card

Used on account/billing settings pages. Three content states — same component structure, different copy.

```
plan-card
  plan-card-heading  →  "Current Plan"  (text-2xl, weight-bold)
  plan-card-body
    plan-card-name   →  "Your Current Plan is X"  (text-md, weight-semibold)
    plan-card-desc   →  description  (text-sm, text-muted)
    plan-card-price  →  "USD 0.00 / Per month"  (text-sm, weight-medium)
    btn btn-primary  →  "Manage Your Subscription"  (align-self: flex-start)
```

| State | Plan name copy | Price copy |
| --- | --- | --- |
| Free | Your Current Plan is Free | USD 0.00 / Per month |
| Premium | Your Current Plan is Premium | USD 99.00 / Per month |
| Enterprise | Your Current Plan is Enterprise | Custom pricing / Per year |

CSS: `bg-surface; border-thin border-default; radius-xl; shadow-card; padding spacing-xl; max-width 680px`

---

### Change Password Card

Settings surface. Uses the same `plan-card` container.

Layout:
- Full-width **Current Password** input on its own row
- **New Password** + **Confirm New Password** in a `display: grid; grid-template-columns: 1fr 1fr; gap: spacing-lg` row
- **Password Requirements** bold label + bullet list (`text-sm`, `text-muted`)
- **Save Changes** primary button, `align-self: flex-start`

All inputs: `type="password"` with correct `autocomplete` attributes.

---

### Two-Factor Authentication Card

Settings surface. Uses the same `plan-card` container (`max-width: 860px`).

**Disabled state (2FA off):**
- Heading: "Two Factor Authentication"
- Subtitle: muted `text-sm` below heading
- Alert banner: `background: #FEF2F2; border-radius: radius-md; padding: spacing-md spacing-lg`
  - Text: "You have not enabled two factor authentication." — `color-error`, `weight-semibold`
- Body copy: `text-sm`, `text-body`, `leading-relaxed`
- **Enable** button → opens confirm-password modal (`open2fa()`)

**Confirm Password modal** (triggered by Enable):
- Overlay: `modal-overlay` class, click-outside closes
- Modal container: `radius-xxl`, `shadow-default`, `padding spacing-2xl`, `max-width 540px`
- Close button: `modal-close` — circular 36px, `radius-full`, top-right absolute
- Title: "Confirm Password" (`text-2xl`, `weight-bold`)
- Subtitle: "For your security…" (`text-sm`, `text-muted`)
- Password input (auto-focused on open)
- Actions row: `justify-content: flex-end; gap: spacing-sm` — **Cancel** (`btn-ghost`) + **Confirm** (`btn-primary`)
- Animation: same `modal-enter` / `modal-exit` keyframes as premium modal (280ms / 200ms)

---

### Paginator

**Design rules:**
- Previous / Next shown only when list has **> 5 pages**
- First and last page numbers always visible (users need to know total pages)
- Ellipsis (`…`) fills gaps in long page lists: `1 2 3 … 28 29 30`
- **Previous disabled** when on page 1
- **Next disabled** when on last page
- Active page: `brand-primary-default` fill, white text

**Previous button states:**

| State | Style |
| --- | --- |
| Default | transparent bg, `text-body` |
| Outlined | `border-default` border, `text-body` |
| Active | `brand-primary-default` fill, white text |
| Tint | `brand-primary-tint` bg, `brand-primary-default` text |
| Disabled | `text-disabled`, no bg, `pointer-events: none` |

**Token reference:**
- `.pg-btn` — `height: 32px; min-width: 32px; radius-md; bg-selected; text-sm; weight-medium`
- `.pg-btn.is-active` — `brand-primary-default` bg, white text
- `.pg-btn:disabled` — `text-disabled`, transparent bg
- `.pg-nav` — transparent bg, horizontal padding `spacing-sm`
- `.pg-ellipsis` — `text-muted`, non-interactive

**Sizes:**

| Modifier | Height |
| --- | --- |
| `.paginator-sm` | 24px |
| `.paginator` (default) | 32px |
| `.paginator-md` | 40px |
| `.paginator-lg` | 48px, `radius-full` on page buttons |

**Row layout with result count:**
```html
<div class="pg-row">  <!-- border-top, space-between -->
  <span class="pg-count">Showing 41–60 of 580 results</span>
  <nav class="paginator">…</nav>
</div>
```

---

### Citation Panel

Appears below chat message bubbles. Bottom corners rounded only (`border-radius: 0 0 radius-md radius-md`).

**Three states:**

| State | Content |
| --- | --- |
| Collapsed | Header bar only |
| Internal source | Header + citation row (file icon + blue link + domain URL) |
| External source | Header + citation row + dark "External source" badge |

**Header bar** (`citation-header`):
- Background: `bg-canvas`
- Left: `ti-info-circle` icon (16px) + "Sources referenced in this response" (`text-sm`, `text-muted`)
- Right: "Reference 1/4" (`text-sm`, `text-muted`)

**Citation row** (`citation-row`):
- Left nav: `citation-nav-btn` — 28px circle, `bg-canvas`, `border-default` border, `ti-chevron-left`
- Content: icon + title link (`text-link` color, `text-sm`, `weight-medium`) + `ti-arrow-up-right` + domain URL below (`text-xs`, `text-muted`)
- Internal source icon: `ti-file-text`
- External source icon: `ti-world`
- External badge: `citation-external-badge` — `text-heading` bg, white text, `radius-sm`, `text-xs`
- Right nav: same 28px circle button, `ti-chevron-right`

---

## VUETIFY — PRIMARY COMPONENT FRAMEWORK (NEW FEATURES)

**Vuetify is the sole UI component provider for all new features.** All Vuetify components are wrapped in custom Vue SFCs in `@/Components/CustomGPT/`. **Always use the custom wrapper, never the raw `v-*` component directly in feature code.**

---

### Component wrapper library (`@/Components/CustomGPT/`)

This is the real component inventory. Use these — do not reinvent them.

**Buttons** — all built on `ButtonBase.vue` → `v-btn`

| Component | Color | Default variant | Notes |
| --- | --- | --- | --- |
| `PrimaryButton` | `primary` | `flat` | Main CTA |
| `SecondaryButton` | `secondary` | `flat` | Secondary action |
| `DangerButton` | `danger` | `flat` | Destructive action |
| `IconButton` | `default` | `text` | Icon-only; renders `<Icon>` inside |
| `ButtonBase` | any | configurable | Base layer — avoid using directly in features |

All buttons accept: `type`, `block`, `disabled`, `flat`, `density`, `variant`, `rounded`.
`PrimaryButton` / `SecondaryButton` / `DangerButton` also accept `prepend` / `default` / `append` slots.

**Inputs**

| Component | Wraps | Default density | Default variant | Notes |
| --- | --- | --- | --- | --- |
| `TextInput` | `VTextField` | `compact` | `outlined` | `flat=true`, `rounded="md"`, `hide-details`, `bg-color="#fff"` |
| `TextInputWithIcon` | `TextInput` | — | — | Adds icon slot |
| `Checkbox` | `VCheckbox` | `compact` | — | `color="primary"`, `hide-details` |
| `CheckboxGroup` | `Checkbox` | — | — | Group wrapper |
| `RadioButton` | — | — | — | |
| `RadioButtonGroup` | — | — | — | |
| `Switch` | `VSwitch` | `compact` | — | `inset`, `color="primary"` |
| `DatePicker` | — | — | — | |
| `TimeInput` | — | — | — | |
| `ColorPicker` / `ColorPickerInput` | — | — | — | |

**Containers**

| Component | Wraps | Notes |
| --- | --- | --- |
| `Card` | `VCard` | `rounded="2xl"`, `density="comfortable"`, `class="shadow-card"` — slots: `title`, `subTitle`, `body`, `actions` |
| `CardWithLoader` | `Card` | Adds loading state |
| `CardWithScrollBox` | `Card` | Scrollable body |
| `Modal` | `VDialog` | `width="500"` default; optional close `IconButton` (`color="white"`) |
| `ModalWithCard` | `Modal` + `Card` | Pre-composed modal+card |
| `ModalWithCardWithLoader` | above + loader | |
| `NavigationDrawer` | `VNavigationDrawer` | |

**Feedback**

| Component | Wraps | Notes |
| --- | --- | --- |
| `Alert` | `VAlert` | Prop: `status` (color name: `"danger"`, `"success"`, etc.) — NOT `type` |
| `SnackBar` | `VSnackbar` | `location="top end"`, `variant="flat"` |
| `Tooltip` | `VTooltip` | See tooltip section below |
| `CircularLoader` | `VProgressCircular` | |
| `ProgressBar` | `VProgressLinear` | |

**Display**

| Component | Notes |
| --- | --- |
| `Chip` | `VChip tag="div"`, `bgCustomOpacity` prop (default 0.12) |
| `ChipWithIcon` | Chip + icon |
| `Tag` | Semantic tag component |
| `Icon` | Tabler icon wrapper — `icon` (name), `size`, `family` |
| `Pagination` | Custom paginator |
| `HorizontalTabs` | Tabs |
| `DropdownMenu` | `VMenu`, `width=225`, `close-on-content-click=false` — slots: `activator`, `default` |
| `DropdownList` | Pre-styled list for use inside `DropdownMenu` |

---

### Theme setup (`plugins/vuetify.js`)

```js
import { createVuetify } from 'vuetify'

export default createVuetify({
  theme: {
    defaultTheme: 'customgpt',
    themes: {
      customgpt: {
        dark: false,
        colors: {
          primary:    '#7367F0',
          secondary:  '#A8AAAE',
          success:    '#28C76F',
          warning:    '#FF9F43',
          danger:     '#EA5455',  // custom key — NOT 'error'
          info:       '#0076E5',  // blue
          background: '#F5F5F5',
          surface:    '#FFFFFF',
          teal:       '#46AEAE',
          orange:     '#F7922F',
          darkyellow: '#D7B300',
          gray:       '#A5A2AD',
          white:      '#FFFFFF',
        },
      },
    },
  },
  defaults: {
    VBtn:       { elevation: 0 },
    VCard:      { elevation: 0 },
    VTextField: { variant: 'outlined', rounded: 'md', density: 'compact', flat: true, hideDetails: true },
    VSelect:    { variant: 'outlined', rounded: 'md', density: 'compact', hideDetails: true },
    VSwitch:    { color: 'primary', density: 'compact', inset: true, hideDetails: true },
    VCheckbox:  { color: 'primary', density: 'compact', hideDetails: true },
  },
})
```

---

### Vuetify rounded prop → radius token map

| Vuetify `rounded` prop | CSS value | Design token | Used on |
| --- | --- | --- | --- |
| `rounded="sm"` | 4px | `radius-sm` | small controls, tooltips |
| `rounded="md"` | 8px | `radius-md` | buttons, inputs |
| `rounded="lg"` | 12px | `radius-lg` | chat bubbles |
| `rounded="xl"` | 16px | `radius-xl` | modals |
| `rounded="2xl"` | 12px | `radius-xxl` | cards (changed from `rounded="card"`) |
| `rounded="pill"` | 9999px | `radius-full` | chips, badges, avatars |

---

### Tooltip — actual production pattern

The `Tooltip` component has two modes controlled by the `parent` prop:

```vue
<!-- Mode 1: parent=true — activator is the parent element -->
<Tooltip parent text="Explain this setting" />

<!-- Mode 2: default — wraps activator in a span, shows help icon -->
<Tooltip text="Explain this setting">
  <template #activator>
    <MyTriggerElement />
  </template>
</Tooltip>

<!-- Suppress the "Chat to learn more" button -->
<Tooltip parent text="Simple tip" :without-ask-ai="true" />
```

Tooltip CSS override (already in `Tooltip.vue` — do not duplicate):
```scss
.tooltip:deep(.v-overlay__content) {
  @apply pointer-events-auto rounded-md border-gray-200 bg-white text-body shadow-card;
}
```
White bg, `shadow-card`, `rounded-md` — never dark.

---

### Alert — actual production pattern

```vue
<!-- status = color key: 'success', 'danger', 'warning', 'info' -->
<Alert status="danger" message="Something went wrong." :closable="true" />

<!-- With slots -->
<Alert status="success">
  <template #title>Saved</template>
  <template #text>Your changes have been saved.</template>
</Alert>
```

`status` maps to a Vuetify `color` — always use production color keys (`danger`, not `error`).

---

### Switch — actual production pattern

```vue
<Switch v-model="isEnabled">
  <template #label>Enable feature</template>
</Switch>

<!-- Large size variant -->
<Switch v-model="isEnabled" size="large" />
```

Custom CSS for thumb/track is already in `Switch.vue` — do not re-implement.

---

### Card — actual production pattern

```vue
<Card>
  <template #title>Card heading</template>
  <template #subTitle>Optional subtitle</template>
  <template #body>Content goes here</template>
  <template #actions>
    <PrimaryButton>Save</PrimaryButton>
  </template>
</Card>

<!-- No padding variants -->
<Card :no-body-padding="true" :no-title-padding="true" />
```

`rounded="2xl"` and `class="shadow-card"` are applied automatically — do not add them again.

---

### Modal — actual production pattern

```vue
<Modal v-model="isOpen" width="500" :show-close-button="true">
  <Card>
    <template #body>Modal content</template>
  </Card>
</Modal>

<!-- Or use the pre-composed version -->
<ModalWithCard v-model="isOpen">
  <template #title>Dialog Title</template>
  <template #body>Content</template>
  <template #actions>
    <PrimaryButton>Confirm</PrimaryButton>
  </template>
</ModalWithCard>
```

---

### DropdownMenu — actual production pattern

```vue
<DropdownMenu :width="225" location="bottom">
  <template #activator="props">
    <PrimaryButton v-bind="props">Options</PrimaryButton>
  </template>
  <DropdownList>
    <!-- list items -->
  </DropdownList>
</DropdownMenu>
```

---

### Custom CSS inside Vue SFCs

When Vuetify props don't cover a need, use Tailwind utility classes (they coexist via `important: true`) or design token CSS vars:

```vue
<style scoped lang="scss">
.my-element {
  @apply bg-white shadow-card rounded-xl p-6;
}

/* For Vuetify internals */
:deep(.v-card__text) {
  @apply text-body;
}
</style>
```

---

### Production `--v-theme-*` color reference

| Vuetify key | Hex | Tailwind key |
| --- | --- | --- |
| `primary` | `#7367F0` | `primary.DEFAULT` |
| `secondary` | `#A8AAAE` | `secondary.DEFAULT` |
| `success` | `#28C76F` | `success.DEFAULT` |
| `warning` | `#FF9F43` | `warning.DEFAULT` |
| `danger` | `#EA5455` | `danger.DEFAULT` |
| `info` | `#0076E5` | `info.DEFAULT` (blue) |
| `background` | `#F5F5F5` | body bg |
| `surface` | `#FFFFFF` | `white` |
| `teal` | `#46AEAE` | `teal.DEFAULT` |
| `orange` | `#F7922F` | `orange.DEFAULT` |
| `darkyellow` | `#D7B300` | `darkyellow.DEFAULT` |
| `gray` | `#A5A2AD` | `gray.DEFAULT` |

**Text / UI colors (not Vuetify theme keys — use Tailwind or raw):**

| Role | Value | Tailwind class |
| --- | --- | --- |
| Heading | `#212121` | `text-heading` |
| Body | `#565656` | `text-body` |
| Muted | `#B7B5BE` | `text-muted` |
| Placeholder | `#999999` | `text-placeholder` |

**⚠ `danger` not `error`:** Production registers destructive color as `danger`. Vuetify's internal form validation uses `error` — they are separate. Always use `color="danger"` in component props.

---

### Vuetify SCSS configuration (`vuetify.scss`)

Source: `/Users/miodragristovski/Downloads/vuetify.scss`

This file is forwarded to `vuetify/settings` and controls every Vuetify default. Key settings and their practical implications:

#### Global flags
```scss
$reset: false        // Vuetify does NOT reset base styles — Tailwind coexists cleanly
$utilities: false    // Vuetify utility classes disabled — Tailwind handles all utilities
$color-pack: false   // Vuetify built-in color classes disabled — use Tailwind color tokens
```

#### Font
```scss
$body-font-family: 'Public Sans', sans-serif, …
```
Vuetify components render in **Public Sans**. Tailwind `font-sans` = **Inter**. Both coexist — do not override Vuetify's font on its own components.

#### Border radius (`$border-radius-root: 4px`)
The full `$rounded` map derived from the 4px root:

| Vuetify `rounded` prop | Value | Use |
| --- | --- | --- |
| `rounded="sm"` | 4px | small controls, tooltips |
| `rounded` (default) | 6px | card default (`$card-border-radius`) |
| `rounded="md"` | 6px | inputs, buttons |
| `rounded="lg"` | 8px | — |
| `rounded="xl"` | 10px | — |
| `rounded="xxl"` | 12px | — |
| `rounded="xxxl"` | 14px | — |
| `rounded="pill"` | 9999px | chips, badges, avatars |
| `rounded="circle"` | 50% | avatar circles |

#### Card
```scss
$card-elevation: 6              // cards have elevation 6 by default
$card-border-radius: 6px        // SCSS default; Card.vue overrides with rounded="2xl" (12px)
$card-text-padding: 24px        // VCardText padding = spacing-xl
$card-item-padding: 24px        // VCardItem padding = spacing-xl
$card-actions-padding: 0 24px 12px   // actions: 0 top, 24px sides, 12px bottom
$card-actions-min-height: unset // no enforced min-height on actions
$card-subtitle-opacity: 1       // subtitles are full opacity (not dimmed)
```
**Important:** `$card-elevation: 6` means VCard has a shadow by default. The `class="shadow-card"` on the `Card` component wins via Tailwind's `!important`. Never fight this with `elevation: 0` in feature code — use the `Card` wrapper which already handles it.

#### Button
```scss
$button-height: 40px
$button-elevation: ('default': 2, 'hover': 4, 'active': 2)
$button-margin-start: 0
$button-margin-end: 0
```
Buttons have elevation 2 by default (not 0). The demo's `VBtn: { elevation: 0 }` default overrides this. In production, button shadow is present — do not fight it; use `variant="flat"` to suppress elevation on specific buttons when needed.

#### Dialog
```scss
$dialog-card-header-padding: 20px 24px 0
$dialog-card-text-padding: 20px 24px 20px
```
Modal header = 20px top, 24px sides. Modal body = 20px top/bottom, 24px sides. These values come from the SCSS — do not add extra padding to `Modal` + `Card` body content.

#### Switch (inset dimensions)
```scss
$switch-track-opacity: 1
$switch-inset-track-height: 1.125rem    // 18px
$switch-inset-track-width: 1.875rem     // 30px
$switch-inset-thumb-width: 0.75rem      // 12px
$switch-inset-thumb-height: 0.75rem     // 12px
$switch-thumb-offset: -2px
```
Switch dimensions are tightly specified. Do not override thumb/track sizes via CSS — the `Switch.vue` component already handles custom styling.

#### Tooltip (raw Vuetify default — NOT what `Tooltip.vue` renders)
```scss
$tooltip-background-color: #212121     // DARK background
$tooltip-text-color: white             // white text
$tooltip-font-size: 0.875rem           // 14px
$tooltip-border-radius: 6px
```
⚠ **Raw `VTooltip` is dark by default.** The production `Tooltip.vue` wrapper overrides this via scoped SCSS to white bg + `shadow-card`. Always use the `Tooltip` component wrapper — never raw `VTooltip`.

#### Alert
```scss
$alert-title-font-size: 1.125rem    // 18px
$alert-title-font-weight: 600
$alert-border-opacity: 0.38
$alert-prepend-margin-inline-end: 0.75rem
```

#### Tabs
```scss
$tabs-height: 42px
```

#### Snackbar
```scss
$snackbar-background: #212121    // dark default
```
The `SnackBar.vue` passes `color` prop which overrides this — use status colors (`success`, `danger`, etc.) always.

#### Grid breakpoints
```scss
$grid-breakpoints: (xs: 0, sm: 576px, md: 768px, lg: 992px, xl: 1200px, xxl: 1400px)
```
Matches Tailwind `screens` config exactly. Vuetify responsive props (`sm:`, `md:`, etc.) use the same breakpoints as Tailwind utility classes.

---

### Vuetify anti-patterns (FORBIDDEN)

- Using raw `<v-btn>`, `<v-card>`, etc. directly in feature code — always use the custom wrapper
- `color="error"` — use `color="danger"` instead
- `variant="elevated"` on buttons — always `variant="flat"` or `variant="outlined"`
- Setting `color` prop to a raw hex string — use named theme color keys only
- Dark tooltips — always white bg via `.tooltip:deep(.v-overlay__content)` pattern
- `elevation` > 0 on cards — use `class="shadow-card"` via the `Card` component
- Omitting `hide-details` on inputs/checkboxes unless validation messages are needed
- Hardcoding `border-radius` px values — use `rounded="*"` prop or Tailwind radius classes
- Vuetify's default `text-transform: uppercase` on buttons — always add to global CSS: `.v-btn { text-transform: none !important; letter-spacing: normal !important; }`
- Setting `elevation > 0` on `VBtn` globally — ghost/outlined/text buttons must never have shadow. Always use `VBtn: { elevation: 0 }` in defaults; apply `shadow-cta` via Tailwind class explicitly on CTAs only when needed.
