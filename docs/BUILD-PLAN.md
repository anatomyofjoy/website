# Build Plan — Anatomy of Joy Shopify Theme

## Base Theme
**Shopify Dawn** (latest) — Shopify's open-source reference theme. Cleanest codebase, best AI comprehension, latest architecture (sections everywhere, JSON templates).

## Reference Documents
- `docs/AOJ-BRAND-STYLE-GUIDE-RAW.md` — Shivantika's brand guide (source of truth)
- `docs/BRAND-BRIEF.md` — Compiled brief with product context and build notes
- `docs/DESIGN-REFERENCE.md` — Whiled.co analysis, animation rules, layout principles

## Available Assets
- `assets/logo/` — 5 logo files (AOJ Smiley logomark, Wordmark, transparent/cropped variants)
- `assets/fonts/DM_Sans/` — Body font (variable + static weights)
- `assets/fonts/Pasta and Wine/Regular Fonts/` — Display font (Big + Little .otf)
- `assets/fonts/Pasta and Wine/SVG Fonts/` — Display font SVG variants
- `assets/fonts/Infamous-Messy-Handwriting-Font/` — Creative display font (.otf)
- `assets/images/` — 53 product photos (scarf mockups, lifestyle shots, artboards, 2 videos)
- `assets/dividers/` — 8 hand-drawn divider PNGs (dots, lines, organic shapes)
- `assets/arrows/black/` — 13 black arrow/line PNGs (thin + thick, various directions)
- `assets/arrows/white/` — 21 white arrow/line/artwork PNGs

---

## Color System

| Token | Hex | Usage |
|-------|-----|-------|
| `--color-ink` | `#161616` | Primary text, dark elements |
| `--color-canvas` | `#f6f3ee` | Page background (warm cream) |
| `--color-yellow` | `#f4ba16` | Accent, logomark, highlights, hover states |

Dawn's default palette will be replaced entirely with these three colors. No additional accent colors.

## Typography System

| Role | Font | Weight/Style | Usage |
|------|------|-------------|-------|
| **Display** | Pasta and Wine Big | Regular | h1, h2, hero text, section headers |
| **Display Alt** | Pasta and Wine Little | Regular | Subheads, pullquotes, smaller display contexts |
| **Body** | DM Sans | Regular (400), Medium (500) | Paragraphs, descriptions, navigation, UI labels |
| **Body Emphasis** | DM Sans | SemiBold (600) | Buttons, CTAs, prices |
| **Creative** | Infamous | Regular | Social media contexts only (not used on website) |
| **Logo Script** | Custom (logo PNGs only) | N/A | Logo wordmark — used as image, never as typed text |

### Font Loading Strategy
- Load `Pasta and Wine Big.otf` and `Pasta and Wine Little.otf` as custom @font-face
- Load DM Sans via variable font file (`DMSans-VariableFont_opsz,wght.ttf`) for smallest payload
- Fallback stack: `DM Sans, -apple-system, BlinkMacSystemFont, sans-serif`
- Display fonts: `'Pasta and Wine Big', Georgia, serif`

## Animation Rules (from Design Reference)

**CSS transitions only. No JavaScript animation libraries.**

- Hover transitions: 0.25-0.35s ease
- Button hover: background + color transition
- Image hover: opacity fade for secondary image
- Cart drawer: opacity + transform slide
- Mobile nav: full-screen opacity overlay
- **Zero scroll-triggered animations**
- **Zero parallax**
- **One decorative animation max** (optional: slow marquee or ticker)

---

## Pages & Sections

### 1. Homepage (`/`)

**Flow (top to bottom):**

1. **Hero Section**
   - Full-width, warm canvas background
   - AOJ Wordmark (logo image, not text)
   - Tagline: "where your stories find a forever."
   - Single CTA button → Shop / How It Works
   - One hero lifestyle image (scarf being worn or laid flat)

2. **Brand Story Teaser**
   - Short paragraph introducing AOJ's mission
   - "At Anatomy of Joy, we create personalized, illustrated keepsakes..."
   - Hand-drawn divider (`assets/dividers/`) between sections
   - Arrow (`assets/arrows/black/`) pointing to next section

3. **How It Works** (3 steps)
   - Step 1: Share your story / location
   - Step 2: We illustrate it by hand
   - Step 3: Receive your wearable keepsake
   - Each step with a small icon or illustration
   - This is critical UX — explains the custom order flow

4. **Featured Scarves**
   - 2-3 scarf images from `assets/images/` (product shots)
   - Each with a story snippet ("Susan's NYC neighborhood" etc.)
   - Hover: subtle opacity transition on image
   - CTA: "See more stories" → collection page

5. **Testimonial / Social Proof**
   - "Featured in Etsy's Top 100 Gifts 2025"
   - Customer quote(s) if available
   - Star rating reference
   - Hand-drawn frame from visual library wrapping the testimonial

6. **Our Story Teaser**
   - Shivantika's photo or illustration
   - Short founder note
   - CTA → Our Story page
   - Hand-drawn arrow pointing to link

7. **Newsletter Signup**
   - Email input + submit
   - "Join our story" or similar CTA
   - Warm, minimal design

8. **Footer**
   - AOJ Smiley logomark
   - Navigation links
   - Social (Instagram, Etsy)
   - Copyright

### 2. Shop / Collection Page (`/collections/all`)

- Grid of scarf products (2 columns desktop, 1 mobile)
- Each product card: image, name, price ($195), "Customize" CTA
- Hand-drawn divider between header and grid
- Filter by type if needed later (Story Scarves, Framed)

### 3. Product Page (`/products/:handle`)

- Large image gallery (slider, multiple angles/mockups)
- Product title in Pasta and Wine display font
- Price
- "How It Works" inline explainer (the custom order steps)
- Customization fields: location/story description, special requests
- Add to Cart → starts the creative collaboration
- Product description (editorial tone, not spec sheet)
- Related products or "More stories"

### 4. Our Story (`/pages/our-story`)

- Shivantika's story and the origin of AOJ
- Brand pillars (Story, Keepsake, Blank Canvas)
- Process photos showing illustration work
- Hand-drawn dividers between sections
- Editorial layout with text + images side by side

### 5. FAQ (`/pages/faq`)

- Accordion-style Q&A
- Topics: How custom ordering works, materials, shipping, returns, care instructions
- Clean DM Sans body text

### 6. Cart Drawer (slide-out)

- Right-side slide-out panel
- Product thumbnail, name, price, quantity
- Subtotal
- "Proceed to Checkout" button
- Checkout → Shopify's hosted checkout

---

## Visual System — How AOJ Assets Map to Sections

### Dividers (`assets/dividers/`)
Used between major homepage sections and as decorative separators on content pages. Rotate through the 8 variants. Implemented as `<img>` elements with `max-width` constraints, centered.

### Arrows (`assets/arrows/black/`)
Used as editorial accents — pointing to CTAs, highlighting testimonials, drawing attention to "How It Works" steps. Positioned with CSS as decorative overlays. Think magazine layout annotations.

### Logo
- **Wordmark** (`AOJ Wordmark.png`): Header, hero, formal contexts
- **Smiley** (`AOJ Logo_Cropped.png`): Footer, favicon, casual/repetitive contexts
- **Never used together** (per brand guide)

---

## Technical Implementation

### Dawn Customizations
1. **Replace Dawn's CSS variables** with AOJ color tokens
2. **Add @font-face declarations** for Pasta and Wine + DM Sans variable font
3. **Override Dawn's typography** classes to use the AOJ type hierarchy
4. **Create custom sections:** hero, how-it-works, testimonial, story-teaser, newsletter
5. **Customize existing sections:** product grid, product page, header, footer
6. **Add AOJ dividers/arrows** as Shopify asset files, referenced in section templates
7. **Implement slide-out cart drawer** (Dawn has this, customize styling)
8. **Custom product form** with order notes for personalization details

### Theme Architecture
```
theme/
├── assets/           # CSS, JS, fonts, images, dividers, arrows
├── config/           # Theme settings (colors, typography, social)
├── layout/           # theme.liquid (base layout)
├── locales/          # i18n strings
├── sections/         # All page sections (modular)
├── snippets/         # Reusable Liquid partials
├── templates/        # Page templates (JSON)
└── templates/customers/  # Account pages
```

### Key Files to Customize from Dawn
- `layout/theme.liquid` — Add font loading, update meta
- `assets/base.css` — Replace color palette, typography
- `sections/header.liquid` — AOJ wordmark, simplified nav
- `sections/footer.liquid` — AOJ smiley, minimal links
- `sections/main-product.liquid` — Custom order fields, editorial layout
- `templates/index.json` — Homepage section order
- New: `sections/aoj-hero.liquid`
- New: `sections/aoj-how-it-works.liquid`
- New: `sections/aoj-story-teaser.liquid`
- New: `sections/aoj-testimonial.liquid`
- New: `sections/aoj-newsletter.liquid`

---

## Build Phases

### Phase 1: Foundation
- Pull Dawn theme into repo
- Replace color palette + typography
- Load custom fonts
- Set up header with wordmark + minimal nav
- Set up footer with smiley + links
- Warm canvas background everywhere

### Phase 2: Homepage
- Build all custom sections (hero, how-it-works, featured, testimonial, story teaser, newsletter)
- Integrate dividers between sections
- Place arrows as decorative accents
- Responsive design (mobile-first)

### Phase 3: Shop & Product
- Customize collection grid (2-col, editorial feel)
- Build product page with custom order flow
- Add personalization fields (location, special requests)
- Image gallery styling

### Phase 4: Content Pages
- Our Story page layout
- FAQ page with accordion
- Contact/custom order inquiry

### Phase 5: Polish
- Hover animations (CSS transitions only, 0.25-0.35s)
- Cart drawer styling
- Mobile responsiveness pass
- Favicon (AOJ Smiley)
- Open Graph / social meta images
- SEO meta templates

---

## What Shivantika Manages After Launch (via Shopify Admin)
- Product photos and descriptions
- Blog/journal posts (if added later)
- Homepage section text (via theme editor)
- Product inventory and pricing
- Order management
- Discount codes / promotions

## What Needs Claude Code (structural changes)
- New section types
- Layout changes
- Font or color system updates
- New page templates
- Animation behavior changes
