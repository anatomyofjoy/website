# Design Reference — Anatomy of Joy

Primary inspiration: [whiled.co](https://whiled.co)

## Architecture Decision

**Shopify with a custom theme** (not headless). Whiled.co proves a Shopify theme can be editorial, brand-forward, and beautiful. For a non-technical founder managing via AI + Shopify's editor, keeping everything in one system is the right call.

- Shopify handles: hosting, SSL, checkout, payments, inventory, orders, shipping, responsive design, security
- Theme handles: brand experience, layout, typography, color, editorial content
- AI + Shopify editor handles: ongoing changes and maintenance

---

## Typography System (3 fonts, strict hierarchy)

Whiled uses three fonts, each with a clear role. Anatomy of Joy should follow the same pattern:

| Role | Whiled's Font | Usage |
|------|--------------|-------|
| Display/Headlines | **Meek** (serif) | h1, h2, h3, hero text, product names |
| Body/Editorial | **Freight** (serif) | Descriptions, stories, quotes, blog content. Often italic for editorial voice |
| System/UI | **Beatrice** (sans-serif) | Navigation, labels, CTAs, prices. Always uppercase, letter-spacing 0.08em-1.2px |

**Key rules:**
- Display font = warmth and personality
- Body font = readability and editorial feel
- UI font = structure and clarity
- Never mix roles. Headlines don't use the UI font. Labels don't use the display font.

*Anatomy of Joy will need its own font selections that match the brand. The 3-role structure stays the same.*

---

## Color Palette

Whiled's palette (for reference, not to copy):
- **Background:** `#f5ebe8` (warm pinkish beige — NOT white)
- **Red:** `#ef4431` (primary accent)
- **Blue:** `#2960ad` (secondary accent)
- **Yellow:** `#ffc500` (tertiary, hover states)
- **Black:** text

**Design principles from the palette:**
- Warm background, never pure white. Sets the entire mood.
- 3 accent colors used structurally (as border colors, dividers, hover states) — not decoratively
- Different sections get different colored borders, creating a subtle color-coding system
- Hover states use a single accent color (Whiled uses yellow)

*Anatomy of Joy needs its own palette. Should reflect the brand's identity — likely warm, artisanal, possibly earthy or jewel-toned given hand-crafted scarves.*

---

## Animation Philosophy (Critical — What Makes It Not Tacky)

### The Rules
1. **CSS transitions only.** No GSAP, no Lottie, no AOS, no scroll-triggered animation libraries.
2. **Hover-driven, not scroll-driven.** Nothing fades in as you scroll. Everything is visible immediately.
3. **Fast transitions: 0.25s-0.35s.** Nothing lingers or overshoots.
4. **One animation pattern per element type.** Buttons do one thing. Images do one thing. No stacking effects.
5. **No parallax. No page transitions. No loading animations.**

### Specific Patterns
- **Buttons:** `background` and `color` transition on hover (0.3s)
- **Navigation links:** `color` transition on hover (0.25s)
- **Product images:** Second image fades in on hover (`opacity 0.3s`)
- **Add-to-cart overlay:** `translateY` slide up from bottom on hover (0.3s, cubic-bezier ease-out)
- **Logo:** Fill color transition on hover (0.3s)
- **Cart/drawer:** `opacity` transition for show/hide
- **Mobile nav:** Full-screen overlay with `opacity 0.25s`
- **Lazy images:** Simple `opacity 0→1` when loaded. No slide, no scale.

### The One Exception
A single decorative animation is acceptable: Whiled uses a **footer marquee ticker** (CSS keyframe, 55s infinite linear loop). Slow, ambient, in a low-attention area. This is the ceiling for decorative motion.

### Anti-Patterns (Never Do)
- Fade-in-on-scroll for content sections
- Parallax backgrounds
- Bouncing/spring/elastic easing
- Staggered animation sequences
- Scale-up transitions on images
- Loading skeleton screens
- Page transition animations
- Text typing/reveal effects
- Counter/number animations

---

## Layout Principles

### Section Structure
- **Colored borders as dividers** between sections — not whitespace gaps or grey lines. Each border can be a different accent color. This is the signature visual device.
- **Full-width sections** with a max-width container (1920px on Whiled)
- **Large editorial image sections** with text overlay
- **Generous vertical padding:** 6rem on desktop, 2rem on mobile
- Container padding: 3rem desktop, 1rem mobile

### Grid & Columns
- Product grid: flexible column splits (not rigid 3x3 or 4x4)
- Asymmetric layouts encouraged (text 40% / image 60%, etc.)
- Mobile: single column, full-width images

### Navigation
- Sticky header with transparent→solid background on scroll
- Clean horizontal nav on desktop
- Full-screen hamburger overlay on mobile (opacity transition)
- Centered logo

### Cart
- **Slide-out drawer cart**, not a separate cart page
- Checkout remains Shopify-hosted (battle-tested, PCI compliant)

---

## Site Structure

Based on Whiled's structure, adapted for Anatomy of Joy:

| Page | Purpose |
|------|---------|
| **Home** | Brand hero, values/philosophy, featured products woven into narrative, founder intro |
| **Shop / Collections** | All scarves, filterable by collection/type/material |
| **Our Story** | Shivantika's story, the craft, inspiration, process |
| **Journal / Blog** | Stories behind designs, process photos, artist features, styling guides |
| **FAQ** | Shipping, care instructions, custom orders, returns |
| **Cart (drawer)** | Slide-out cart → Shopify checkout |

### Homepage Flow (top to bottom)
1. Hero with brand ethos (not product-first)
2. Brand values / philosophy section
3. Featured products woven into editorial content
4. Our Story teaser with founder image
5. Journal/blog teaser
6. Newsletter signup
7. Footer

---

## Product Page Patterns

From Whiled:
- Full-width product image slider (hover arrows, not dots)
- Product info alongside image (not below)
- Inline variant selection
- Editorial product description (not a spec sheet)
- "You may also like" section with editorial treatment
- Price displayed in the serif body font, not the UI font

---

## Mobile Considerations

- Full-width images, no side padding on product images
- Stacked layout (image above text)
- Larger touch targets
- Simplified navigation (hamburger → full overlay)
- Add-to-cart buttons always visible on mobile (not hover-dependent)
- Smaller type sizes but same font hierarchy

---

## Content Voice Notes

Whiled's content voice (for reference):
- Brand-first, product-second
- Mission statement leads ("Our mission is simple: to help the world slow down")
- Founder's personal note on the story page
- Blog features the artists/makers, not just products
- Spotify playlist integration (lifestyle, not just commerce)
- Email signup framed as community ("Daydream with us")

*Anatomy of Joy should develop its own voice. The structural pattern: lead with why, not what. Feature the maker and the craft. Create a world, not just a store.*

---

## Technical Notes

### Whiled's Actual Stack
- Shopify (confirmed via headers: `powered-by: Shopify`)
- Custom theme: "bggy — Dev [10.26.20]" (heavily customized, not an off-the-shelf theme)
- Shopify theme ID: 114564726935
- Uses Vue.js-style `[v-cloak]` directive (likely Vue for interactive components)
- Tailwind CSS utility classes (customized build)
- Lazy loading via `lazysizes` pattern
- Marquee via CSS keyframes (no library)
- No external animation libraries
- Cloudflare CDN (fronting Shopify)
- Klaviyo for email marketing
- Facebook Pixel + Google Ads tracking
- NoFraud for payment fraud detection

### For Anatomy of Joy Build
- Start from a minimal Shopify theme (Dawn or similar) and customize heavily
- Or build a custom theme from scratch using Shopify CLI
- Use Tailwind CSS for utility classes
- Keep JavaScript minimal — CSS transitions for all animation
- Consider Alpine.js or vanilla JS for interactive components (cart drawer, mobile nav)
