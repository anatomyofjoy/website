# Build Plan — Anatomy of Joy Shopify Theme (Design-Reviewed)

## 0) Source of Truth
- Brand: `docs/AOJ-BRAND-STYLE-GUIDE-RAW.md`
- Design behavior + interaction baseline: `docs/DESIGN-REFERENCE.md`
- Platform: Shopify Dawn (latest stable), customized heavily

## 1) Product Intent
Anatomy of Joy is a story-first custom keepsake brand. The site must feel warm, human, and premium, while making the custom order process obvious and low-friction.

Primary UX goal: user understands "how custom scarf creation works" within 10 seconds of landing.

---

## 2) Design System (Locked)

### 2.1 Color Tokens
- `--color-ink: #161616`
- `--color-canvas: #f6f3ee`
- `--color-yellow: #f4ba16`

Derived utility tokens:
- `--color-canvas-strong` (slightly darker canvas tint for panels/bands)
- `--color-ink-20` (borders/dividers)
- `--focus-ring: #f4ba16`

Rule: no new accent hues. Contrast and hierarchy come from tint, line weight, texture, and typography.

### 2.2 Typography Roles
- Display: Pasta and Wine Big (H1/H2 hero and section titles)
- Display Alt: Pasta and Wine Little (short subheads/pull quotes)
- Body + UI: DM Sans (all paragraph, nav, forms, product data)

UI treatment rule (from design reference structure):
- Nav/buttons/labels in DM Sans, uppercase, light tracking (`0.06em-0.1em`)

### 2.3 Type Scale
- H1: clamp(2.2rem, 6vw, 5rem)
- H2: clamp(1.8rem, 4vw, 3.2rem)
- H3: clamp(1.3rem, 2.5vw, 2rem)
- Body L: 1.125rem
- Body M: 1rem
- UI S: 0.75rem-0.875rem uppercase

### 2.4 Spacing & Layout
- Section vertical rhythm: desktop 96px, tablet 72px, mobile 40px
- Container: max-width 1280px, horizontal padding 48px desktop / 20px mobile
- Grid: 12-col desktop, 6-col tablet, 1-col mobile
- Border motif: 1-2px ink/yellow separators, used structurally between major sections

### 2.5 Visual Library Rules (Critical)
- Dividers: max 1 per section boundary
- Arrows: max 2 per viewport, never overlap body copy or controls
- Decorative assets must not carry information needed for comprehension
- Wordmark and smiley never appear together in same block

### 2.6 Texture Use
Use subtle canvas/paper texture overlays in low-contrast areas only (hero bands, testimonial card backgrounds). Never reduce text contrast.

---

## 3) Motion System (Strict)
- CSS transitions only
- Standard durations:
  - Fast: 180ms (links/icons)
  - Base: 280ms (buttons/cards/images)
  - Drawer/overlay: 320ms
- Easing: `ease` or gentle custom cubic-bezier, no bounce/spring
- No scroll-triggered reveals, no parallax, no staged intro animations
- Respect `prefers-reduced-motion`: disable decorative and hover transforms

---

## 4) Page Architecture

### 4.1 Homepage (`/`)
Order:
1. Hero (brand ethos + single primary CTA)
2. How It Works (3-step custom process)
3. Featured Stories (product/story cards)
4. Social Proof (press + customer quote)
5. Founder Teaser
6. Newsletter
7. Footer

Section-specific requirements:
- Hero: one strong image, text left/center depending crop, CTA always visible above fold on mobile
- How It Works: icon + title + one-line explanation per step, no paragraph walls
- Featured Stories: 2-up desktop, 1-up mobile, optional secondary hover image desktop only
- Proof block: framed card + source attribution line
- Founder teaser: portrait + short narrative + "Read Our Story" CTA

### 4.2 Collection (`/collections/all`)
- 2 columns desktop, 1 mobile
- Card anatomy: primary image, title, price, "Customize" CTA
- Empty state: "No stories found yet" + reset filters CTA
- Loading/skeleton state and fallback image handling required

### 4.3 Product (`/products/:handle`)
Two-column desktop, stacked mobile.
Required modules:
- Gallery
- Product identity (title/price/trust line)
- Customization form:
  - Story/location field (required)
  - Optional special requests
  - Validation + inline error copy
- "How this works" mini explainer
- Add to cart + expected timeline note
- Related stories

Mobile:
- Sticky add-to-cart zone
- Form labels always visible (no placeholder-only UX)

### 4.4 Our Story (`/pages/our-story`)
Narrative blocks alternating text/image, clear start-middle-end:
- Origin
- Process
- Values
- Closing invitation to shop or start custom story

### 4.5 FAQ (`/pages/faq`)
Accordion with:
- Keyboard accessible toggles
- Visible open/closed states
- Grouping by topic (ordering, shipping, care, returns)

### 4.6 Cart Drawer
- Right-side drawer desktop, full-screen modal on small mobile
- Visible subtotal + checkout CTA pinned
- Error state: inventory unavailable / quantity issue
- Recovery action always present

---

## 5) Component States (Must Implement)
For every interactive surface:
- Default
- Hover (desktop only)
- Focus-visible
- Disabled
- Loading
- Error
- Success/confirmation (where relevant)

Specific requirements:
- Newsletter: success + error copy variants
- Product personalization: required-field validation before ATC
- Cart: line-item update pending and failure message
- Image blocks: missing image fallback treatment

---

## 6) Accessibility Baseline
- WCAG AA contrast for body + UI text
- Minimum touch target 44x44
- Keyboard navigation for header, drawer, accordions, product form
- Focus rings use yellow token with visible offset
- Semantic heading order (single H1 per template)
- Motion fallback with `prefers-reduced-motion`

---

## 7) Dawn Implementation Scope

### 7.1 Existing Dawn files to customize
- `layout/theme.liquid`
- `assets/base.css`
- `sections/header.liquid`
- `sections/footer.liquid`
- `sections/main-product.liquid`
- `templates/index.json`

### 7.2 New sections
- `sections/aoj-hero.liquid`
- `sections/aoj-how-it-works.liquid`
- `sections/aoj-featured-stories.liquid`
- `sections/aoj-testimonial.liquid`
- `sections/aoj-founder-teaser.liquid`
- `sections/aoj-newsletter.liquid`

### 7.3 Snippets to add
- `snippets/aoj-divider.liquid`
- `snippets/aoj-arrow.liquid`
- `snippets/aoj-story-card.liquid`
- `snippets/aoj-state-message.liquid`

---

## 8) Build Phases

### Phase 1: Foundation
- Import Dawn
- Tokenize color/type/spacing/motion in CSS variables
- Wire font loading
- Header/footer brand integration
- Accessibility primitives (focus, skip link, semantic landmarks)

### Phase 2: Homepage
- Build all homepage sections in final order
- Add visual library with density rules
- Implement all required states
- Mobile pass for every section before moving on

### Phase 3: Collection + Product
- Build story-card collection system
- Build PDP customization flow with validation + trust copy
- Build related stories + fallback states
- Sticky ATC on mobile

### Phase 4: Editorial Pages
- Our Story narrative layout
- FAQ accessible accordion
- Contact/custom inquiry (if included)

### Phase 5: Polish & QA
- Motion tuning to token system
- Cart drawer UX polish
- Accessibility audit pass
- Performance pass (font payload, image sizing, lazy loading)
- Meta/social/favicons

---

## 9) Not in Scope (Explicitly Deferred)
- Blog/Journal launch (prepare teaser slot only)
- Multi-language localization
- Advanced filtering/faceted search
- Loyalty/referral features
- Replatforming to headless

---

## 10) What Already Exists and Should Be Reused
- Dawn cart drawer foundation
- Dawn product media + variant framework
- Dawn schema/settings patterns for section configurability
- Existing AOJ brand assets (logos, dividers, arrows, imagery, fonts)

---

## 11) Available Assets
- `assets/logo/` — 5 logo files (AOJ Smiley logomark, Wordmark, transparent/cropped variants)
- `assets/fonts/DM_Sans/` — Body font (variable + static weights)
- `assets/fonts/Pasta and Wine/Regular Fonts/` — Display font (Big + Little .otf)
- `assets/fonts/Pasta and Wine/SVG Fonts/` — Display font SVG variants
- `assets/fonts/Infamous-Messy-Handwriting-Font/` — Creative display font (.otf)
- `assets/images/` — 53 product photos (scarf mockups, lifestyle shots, artboards, 2 videos)
- `assets/product-flat/` — 49 flat product images (scarf designs + photorealistic mockups)
- `assets/dividers/` — 8 hand-drawn divider PNGs (dots, lines, organic shapes)
- `assets/arrows/black/` — 13 black arrow/line PNGs (thin + thick, various directions)
- `assets/arrows/white/` — 21 white arrow/line/artwork PNGs

---

## 12) Acceptance Criteria (Design)
- Brand coherence: 3/3 brand tokens correctly applied across templates
- Journey clarity: user can describe custom order flow after first screen + one scroll
- Consistency: no section violates spacing/type/motion token rules
- Accessibility: keyboard + focus + contrast pass on homepage, collection, product, cart
- Mobile: no clipped text/overlaps; all primary CTAs visible without hover behavior
