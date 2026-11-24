---
name: Festive Boutique RTL
colors:
  surface: '#fbf8ff'
  surface-dim: '#d7d8f4'
  surface-bright: '#fbf8ff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f4f2ff'
  surface-container: '#edecff'
  surface-container-high: '#e6e6ff'
  surface-container-highest: '#e0e0fc'
  on-surface: '#181a2e'
  on-surface-variant: '#584143'
  inverse-surface: '#2d2f44'
  inverse-on-surface: '#f1efff'
  outline: '#8b7073'
  outline-variant: '#dfbfc1'
  surface-tint: '#af2b49'
  primary: '#a82544'
  on-primary: '#ffffff'
  primary-container: '#c93f5b'
  on-primary-container: '#fff6f6'
  inverse-primary: '#ffb2ba'
  secondary: '#7d562d'
  on-secondary: '#ffffff'
  secondary-container: '#ffca98'
  on-secondary-container: '#7a532a'
  tertiary: '#a52a45'
  on-tertiary: '#ffffff'
  tertiary-container: '#c6435c'
  on-tertiary-container: '#fff6f6'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#ffd9dc'
  primary-fixed-dim: '#ffb2ba'
  on-primary-fixed: '#400011'
  on-primary-fixed-variant: '#8e0d33'
  secondary-fixed: '#ffdcbd'
  secondary-fixed-dim: '#f0bd8b'
  on-secondary-fixed: '#2c1600'
  on-secondary-fixed-variant: '#623f18'
  tertiary-fixed: '#ffd9dc'
  tertiary-fixed-dim: '#ffb2ba'
  on-tertiary-fixed: '#400011'
  on-tertiary-fixed-variant: '#8b1433'
  background: '#fbf8ff'
  on-background: '#181a2e'
  surface-variant: '#e0e0fc'
typography:
  headline-xl:
    fontFamily: Rubik
    fontSize: 40px
    fontWeight: '700'
    lineHeight: 52px
  headline-xl-mobile:
    fontFamily: Rubik
    fontSize: 30px
    fontWeight: '700'
    lineHeight: 40px
  headline-lg:
    fontFamily: Rubik
    fontSize: 32px
    fontWeight: '600'
    lineHeight: 42px
  headline-lg-mobile:
    fontFamily: Rubik
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
  headline-md:
    fontFamily: Rubik
    fontSize: 22px
    fontWeight: '600'
    lineHeight: 30px
  headline-sm:
    fontFamily: Rubik
    fontSize: 18px
    fontWeight: '500'
    lineHeight: 26px
  body-lg:
    fontFamily: Rubik
    fontSize: 18px
    fontWeight: '400'
    lineHeight: 28px
  body-md:
    fontFamily: Rubik
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
  body-sm:
    fontFamily: Rubik
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
  label-lg:
    fontFamily: Rubik
    fontSize: 15px
    fontWeight: '600'
    lineHeight: 20px
  label-md:
    fontFamily: Rubik
    fontSize: 13px
    fontWeight: '500'
    lineHeight: 18px
  label-sm:
    fontFamily: Rubik
    fontSize: 11px
    fontWeight: '600'
    lineHeight: 14px
    letterSpacing: 0.02em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  gutter: 1rem
  gutter-md: 1.5rem
  gutter-lg: 2rem
  margin: 1rem
  margin-md: 2rem
  margin-lg: 3rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2.5rem
---

## Brand & Style

This design system establishes a warm, boutique, celebratory e-commerce environment crafted specifically for festive gifting in Hebrew (RTL). Combining high-end confectionary aesthetics with joyful balloon decor, the visual language balances artisan craftsmanship with effortless digital shopping.

### Design Principles & Emotional Target
- **Warm Celebration:** The interface feels like stepping into a curated French patisserie crossed with an airy balloon atelier. Whitespace is generous, inviting, and bathed in cream tones rather than sterile digital whites.
- **Boutique Simplicity:** Frictionless, tactile, and intentionally uncluttered. The UI prioritizes visual delight while honoring rapid mobile purchasing workflows.
- **Strict RTL Elegance:** Fully native right-to-left layout dynamics where movement, hierarchy, and affordances guide Hebrew readers naturally from right to left.
- **Zero Human Imagery Policy:** People are never depicted. The atmosphere is carried exclusively by artisanal praline arrangements, textured packaging, metallic helium balloons, silk ribbons, and refined vector illustrations.

## Colors

The palette balances warm confectionary neutrals with celebratory corals and accents of spun gold. All interactive text color combinations strictly satisfy WCAG AA contrast ratios against light backgrounds.

### Color Tokens & Roles

- **Canvas & Surfaces:**
  - `page-bg`: `#FFFBF7` — Warm cream foundation providing a tactile, paper-like warmth.
  - `surface-base`: `#FFFFFF` — Pure white for elevated product cards, dropdowns, and modals.
  - `surface-tint`: `#FFF7F0` — Warm peach-tinted surface for secondary sections, highlight bands, and order summaries.

- **Typography & Content:**
  - `text-primary`: `#2B2D42` — Deep charcoal providing high clarity and softness over harsh pure black.
  - `text-secondary`: `#5A5D75` — Warm muted slate for secondary meta-data, subtitles, and captions.
  - `text-gold`: `#8A6234` — Deepened champagne gold calibrated specifically for legible typography on `#FFFBF7` and `#FFFFFF` (meets 4.5:1 WCAG AA).

- **Brand & Action Accents:**
  - `action-primary`: `#C93F5B` — Rich crimson coral for primary transactional actions, delivery pickers, and checkout CTAs.
  - `action-primary-hover`: `#B03049` — Deep berry for active and hover states.
  - `accent-coral`: `#E85D75` — Vivid coral-pink reserved strictly for non-text decorative badges, festive ribbons, micro-illustrations, and seasonal stickers.
  - `accent-gold`: `#D4A373` — Champagne gold used exclusively for decorative borders, hairline dividers, medal icons, and graphic accents.

- **Structural & Feedback States:**
  - `border-soft`: `#E8DFD8` — Subtle warm champagne-gray for hairline container boundaries and inputs.
  - `state-disabled-bg`: `#F0ECE9` — Subdued warm gray for disabled containers.
  - `state-disabled-text`: `#9496A1` — Muted gray for out-of-stock items and inactive controls.
  - `state-success`: `#2E7D32` — Forest green for completed checkout and delivery slots.
  - `state-warning`: `#D97706` — Honey amber for limited stock warnings.
  - `state-error`: `#D32F2F` — Crisp crimson for form validation errors and delivery date conflicts.

## Typography

The typography system relies on **Rubik**, chosen for its friendly rounded corners, robust Hebrew glyph support, and open counters that remain highly legible at smaller mobile sizes.

### Hebrew Typography Principles
- **Direction:** Applied universally with `dir="rtl"`. Text alignment default is right (`text-align: right`), with numeric price tags and dates aligned contextually to maintain clarity.
- **Letter Spacing:** Kept standard or zeroed out on body and headlines. Unlike Latin typography, Hebrew generally degrades when excessive tracking is applied; subtle tracking is reserved only for micro-caps badges (`label-sm`).
- **Hierarchy:** Weight steps (400 Regular, 500 Medium, 600 SemiBold, 700 Bold) clearly demarcate bundle titles, balloon finish specs (chrome, foil, satin), and pricing units (₪).

## Layout & Spacing

A mobile-first fluid grid anchors the browsing experience, catering to customers shopping on hand-held devices for party gifts.

### Breakpoints & Container Model
- **Mobile (< 640px):** 4-column fluid layout, `margin`: `1rem` (16px), `gutter`: `1rem` (16px). Product listings render as dual-card grids or horizontally scrolling carousels.
- **Tablet (641px - 1024px):** 8-column layout, `margin-md`: `2rem` (32px), `gutter-md`: `1.5rem` (24px).
- **Desktop (> 1025px):** 12-column layout maxing out at `1280px` centered canvas, `margin-lg`: `3rem` (48px), `gutter-lg`: `2rem` (32px).

### RTL Layout Flow
- Grids layout from right to left (`flex-direction: row-reverse` or native CSS logical properties `inline-start` / `inline-end`).
- Breadcrumbs, icon pairings, and directional arrows flip automatically (`transform: scaleX(-1)` for forward chevron navigation).

## Elevation & Depth

Visual depth reflects light bouncing off reflective foil balloons and smooth packaging, relying on soft, diffused ambient light rather than harsh structural drops.

### Elevation Levels
- **Flat (Level 0):** `none` — Used for background panels and seamless inputs resting on `#FFFBF7`.
- **Card Rest (Level 1):** `0 4px 16px -2px rgba(43, 45, 66, 0.05), 0 1px 3px 0 rgba(43, 45, 66, 0.03)` with a hairline border `1px solid #E8DFD8`. Applied to product cards and selection chips.
- **Interactive Hover (Level 2):** `0 12px 28px -4px rgba(201, 63, 91, 0.08), 0 4px 10px -1px rgba(43, 45, 66, 0.04)` with a slight vertical translation (`translateY(-2px)`).
- **Sticky / Overlays (Level 3):** `0 16px 36px -6px rgba(43, 45, 66, 0.12)` for floating cart bars, delivery date pickers, and modal windows.

## Shapes

The design system embraces rounded corners (`8px` base, `16px` for containers, `24px` for specialty cards) reflecting the organic curvature of balloons and confectionery bonbon boxes.

- **Base Radius (8px / `0.5rem`):** Buttons, form inputs, counter steppers, dropdown menus.
- **Container Radius (16px / `1rem`):** Product cards, category tiles, checkout panels, banners.
- **Pill Radius (Full / `9999px`):** Status badges, delivery time tags, quantity increment pills, floating action buttons.

## Components

### Buttons
- **Primary CTA:** Background `#C93F5B`, text `#FFFFFF`, radius `8px`, height `48px` (mobile touch target). Hover: `#B03049`. Subtle scale press feedback (`transform: scale(0.98)`).
- **Secondary / Boutique Outline:** Background `transparent`, border `1.5px solid #D4A373`, text `#8A6234`. Hover: background `#FFF7F0`.
- **Ghost Action:** Background `transparent`, text `#2B2D42`. Hover: background `#FFF7F0`.
- **Disabled State:** Background `#F0ECE9`, text `#9496A1`, border none.

### Product Cards
- Surface `#FFFFFF` encased in a hairline `1px solid #E8DFD8` border with `16px` radius.
- Imagery framed in a 1:1 warm off-white container (`#FFFBF7`).
- Card layout: Badge at top-right (RTL start), quick-add button at bottom-left, product title right-aligned in `headline-sm`, price displayed with Hebrew Shekel symbol (`₪`) in `label-lg` colored `#2B2D42`.

### Badges & Ribbons
- **Bestseller / New:** Background `#E85D75`, text `#FFFFFF`, full-pill radius, text `label-sm`.
- **Praline Quality Badge:** Background `#FFF7F0`, border `1px solid #D4A373`, text `#8A6234`.
- **Stock Alert:** Background `#FFF0F2`, text `#D32F2F`.

### Form Inputs & Selectors
- Background `#FFFFFF`, border `1.5px solid #E8DFD8`, text `#2B2D42`, placeholder `#5A5D75`.
- Focus state: Border `#C93F5B` with subtle glow ring `0 0 0 3px rgba(201, 63, 91, 0.12)`.
- Hebrew label positioned strictly top-right with helper text beneath in `body-sm`.

### Checkboxes & Radios
- Size `20px x 20px`. Border `1.5px solid #E8DFD8`.
- Checked state: Background `#C93F5B` with white checkmark / inner dot.
- Placed on the right-hand side of corresponding option labels (`margin-left: 0.75rem`).

### Greeting Card & Custom Inscription Input
- Special boutique component: Textarea styled with `#FFFBF7` canvas, dashed border `1.5px dashed #D4A373`, with character countdown indicator aligned left.

### Floating Mobile Cart & Delivery Bar
- Fixed bottom mobile bar, background `#FFFFFF` with elevation level 3, top border `1px solid #E8DFD8`.
- Splits summary on right (total amount, delivery estimate) with primary purchase CTA on left.

### Imagery Guidelines
- **Strictly No People:** Never display portraits, hands, or models.
- **Allowed Imagery:** Glossy helium balloon bouquets (metallic, pastel, foil heart shapes), open wooden or velvet praline boxes showing chocolate textures, golden curling ribbons, and delicate confetti accents.