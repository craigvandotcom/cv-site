# craigvan.com - Product Requirements Document

**Version:** 1.0
**Last Updated:** 2026-01-19
**Status:** Ready for Development

---

## Executive Summary

Personal brand website for Craig van Heerden, focused on the Health domain of the neoMeta Life Operating System. MVP launches with EAT pillar (Body Compass) as the primary conversion funnel.

**Domain:** craigvan.com
**Stack:** Astro 5 + Tailwind CSS + Vercel

---

## Vision

Build a minimal, high-converting personal brand website that:
1. Establishes Craig as the "Health Operating System" builder
2. Captures email leads through poster download
3. Directs users to Body Compass app
4. Sets foundation for future pillar expansions

---

## Target Audience

**Primary:** Health-conscious individuals seeking systematic approaches to nutrition
**Secondary:** People interested in personal development systems and life optimization

---

## MVP Scope (V1)

### Pages

| Page | Route | Status |
|------|-------|--------|
| Home | `/` | Build |
| EAT Journey | `/eat` | Build |

**Total: 2 pages**

---

## Page Specifications

### Home Page (`/`)

#### 1. Hero Section

**Layout:** Full viewport height, centered content

**Content:**
- **Headline:** "Craig van Heerden"
- **Tagline:** "eat. move. meditate." (with accent glow on each word)
- **Description:** "Building Life Operating Systems. Start with health sovereignty."
- **Newsletter form:** Email input + "Subscribe" button (SubstackAPI.com)

**Visual Notes:**
- Dark background with subtle green glow effects
- Blueprint aesthetic with fine linework
- See mockups: `v2-hero-v1.png`, `v2-hero-v2.png`

#### 2. Three Pillars Section

**Layout:** Three cards in a horizontal row (stack on mobile)

| Card | Title | Subtitle | Status | Action |
|------|-------|----------|--------|--------|
| EAT | Body Compass | "Nutrition sovereignty" | LIVE (green badge) | Click → `/eat` |
| MOVE | Freedom of Movement | "Pain-free athleticism" | COMING SOON (outlined) | Non-clickable |
| MEDITATE | Art of Stillness | "Equanimity through awareness" | COMING SOON (outlined) | Non-clickable |

**Card Design:**
- Pillar icon (64x64px, from `assets/icons/pillar-*-styled.png`)
- Pillar name (h3)
- Product subtitle (small text)
- Status badge (top-right, absolute positioned)
- Blueprint corner markers (top-left, bottom-right)
- Hover: Border color changes to accent, subtle glow

**Visual Notes:** See mockups: `v2-pillars-v1.png`, `v2-pillars-v2.png`

#### 3. Footer

**Layout:** Three-column flex (stack on mobile)
- **Left:** © 2026 Craig van Heerden
- **Center:** Social icons (X, YouTube, GitHub) - 24px, 16px gap
- **Right:** "neoMeta" wordmark

**Visual Notes:** See mockups: `v2-footer-v1.png`, `v2-footer-v2.png`

---

### EAT Journey Page (`/eat`)

The complete Body Compass conversion funnel. Structure follows: Hero → Problem → Poster → App → Course → Footer.

#### 1. Hero Section

**Content:**
- **EAT pillar icon** (centered, 96x96px)
- **Headline:** "Body Compass"
- **Tagline:** "Nutrition sovereignty. Reclaim your intuitive relationship with food."

**Visual Notes:** Dark background, centered layout, pillar icon prominent

#### 2. The Problem (Optional for V1, can add later)

**Content:**
- Brief pain points this solves
- "You've tried every diet. Nothing sticks."
- "Your body has been trying to tell you something. You just haven't learned to listen."

*Can be skipped for MVP and added in V1.1*

#### 3. Poster Download Section (Primary CTA)

**Layout:** Two columns - poster preview left, form right (stack on mobile)

**Content:**
- **Poster preview:** 3:4 aspect ratio image placeholder (until real poster ready)
- **Headline:** "Get the Body Compass Poster"
- **Description:** "Your visual guide to the red, yellow, and green food zones. Print it, stick it on your fridge, start listening to your body."
- **Form:**
  - Email input (placeholder: "Enter your email")
  - Submit button: "Send Me the Poster"
  - Privacy note: "We'll send the poster PDF to your inbox. No spam, unsubscribe anytime."
- **Success state:** "Check your email! Your poster is on its way."

**Email Automation Flow:**
1. User enters email → submit
2. Email added to Substack (or ConvertKit)
3. Automation sends poster PDF as attachment or download link
4. User enters nurture sequence (weekly newsletter)

#### 4. App Section

**Layout:** Centered content with app screenshot

**Content:**
- **Headline:** "Track with the App"
- **Badge:** LIVE (green, next to headline)
- **Description:** "Body Compass helps you log meals, track reactions, and build food intuition. Discover your personal red, yellow, and green zones."
- **App screenshot:** Phone mockup, 9:16 aspect ratio (placeholder until real screenshots)
- **CTAs:**
  - Primary: "Learn More" → https://bodycompass.app
  - (Future: App Store download buttons when published)

#### 5. Course Section

**Layout:** Centered content

**Content:**
- **Headline:** "Body Compass Course"
- **Badge:** COMING SOON (outlined)
- **Description:** "11 lessons to transform your relationship with food. Learn the complete Body Compass methodology and build lasting food freedom."

*No CTA for now - just informational*

#### 6. Footer
- Same as home page

---

## Design Requirements

### Aesthetic
**Blueprint Web** - Technical precision meets modern UI

### Colors
| Token | Hex | Usage |
|-------|-----|-------|
| Background | `#1a1a1a` | Primary background |
| Text | `#F5F0E6` | Primary text, borders |
| Accent | `#01A878` | Glow effects, highlights |

### Typography
- Headings: Space Grotesk (700)
- Body: Inter (400, 500)
- Mono: JetBrains Mono (code elements)

### Components
- Cards with blueprint corner markers
- Outlined buttons with hover glow
- Email input forms
- Status badges (LIVE, Coming Soon)

**Full specs:** See `SPEC.md`

---

## Technical Requirements

### Stack
| Layer | Choice |
|-------|--------|
| Framework | Astro 5 |
| Styling | Tailwind CSS |
| Hosting | Vercel |
| Newsletter | Substack (via SubstackAPI.com) |
| Analytics | Umami (self-hosted) |

### Integrations

#### Newsletter/Email Capture
- Primary: SubstackAPI.com for custom-styled forms
- Poster automation: Configure in Substack or use separate service (ConvertKit, etc.)

#### Analytics
- Umami on `analytics.craigvan.com`
- Track: page views, poster downloads, app clicks

**Full specs:** See `SPEC.md`

---

## Visual Mockups

**Location:** `/infrastructure/tools/src/image-gen/output/website-mvp/`

| Mockup | Filename | Status |
|--------|----------|--------|
| Hero v1 | `v2-hero-v1.png` | Select favorite |
| Hero v2 | `v2-hero-v2.png` | Select favorite |
| Pillars v1 | `v2-pillars-v1.png` | Select favorite |
| Pillars v2 | `v2-pillars-v2.png` | Select favorite |
| Footer v1 | `v2-footer-v1.png` | Select favorite |
| Footer v2 | `v2-footer-v2.png` | Select favorite |
| Full homepage v1 | `homepage-full-v1.png` | Reference |
| Full homepage v2 | `homepage-full-v2.png` | Reference |

**Decision Needed:** Craig to select preferred variations before build begins.

---

## Assets (Complete)

| Asset | Location | Status |
|-------|----------|--------|
| CV Logo | `assets/logo/cv-logo-light.png` | Ready |
| Favicons | `assets/favicon/` | Ready |
| EAT Icon | `assets/icons/pillar-eat-styled.png` | Ready |
| MOVE Icon | `assets/icons/pillar-move-styled.png` | Ready |
| MEDITATE Icon | `assets/icons/pillar-meditate-styled.png` | Ready |
| Body Compass Poster | TBD | Need PDF (use placeholder for MVP) |
| App Screenshots | TBD | Need from Craig (use placeholder for MVP) |

**Placeholder Strategy:** Use styled placeholder divs with "Poster Preview" and "App Screenshot" text until real assets are provided.

---

## Content Requirements

### Home Page Copy (Finalized)

| Section | Content |
|---------|---------|
| Hero headline | "Craig van Heerden" |
| Hero tagline | "eat. move. meditate." |
| Hero description | "Building Life Operating Systems. Start with health sovereignty." |
| EAT card subtitle | "Nutrition sovereignty" |
| MOVE card subtitle | "Pain-free athleticism" |
| MEDITATE card subtitle | "Equanimity through awareness" |

### EAT Journey Copy (Finalized)

| Section | Content |
|---------|---------|
| Hero headline | "Body Compass" |
| Hero tagline | "Nutrition sovereignty. Reclaim your intuitive relationship with food." |
| Poster headline | "Get the Body Compass Poster" |
| Poster description | "Your visual guide to the red, yellow, and green food zones." |
| App headline | "Track with the App" |
| App description | "Body Compass helps you log meals, track reactions, and build food intuition." |
| Course headline | "Body Compass Course" |
| Course description | "11 lessons to transform your relationship with food." |

**Source:** Content finalized in this document. Original brainstorming in `_archive/` for reference.

---

## Success Metrics

### Primary
- Email captures (poster downloads)
- App store clicks / bodycompass.app visits

### Secondary
- Newsletter signups
- Time on site
- Return visitors

---

## Future Phases

### V2: MOVE Pillar
- `/move` journey page
- Freedom of Movement app integration
- Un-sit Your Back course

### V3: MEDITATE Pillar
- `/meditate` journey page
- Art of Stillness app integration
- Meditation course

### V4: Full Site Expansion
- Philosophy page
- Systems page (12 pillars)
- About page
- Writing/blog

---

## Constraints

1. **No database** - Static site only
2. **Minimal JavaScript** - Astro islands for forms only
3. **Fast load times** - Target Lighthouse 90+
4. **Mobile responsive** - Works on all devices
5. **GDPR compliant** - Umami for analytics (no cookies)

---

## Open Questions

1. **Poster delivery method:**
   - Option A: Substack automation with PDF attachment
   - Option B: Separate email service (ConvertKit, etc.)
   - Option C: Direct download after email capture (no automation)

2. **App screenshots:** Need from Craig

3. **Poster PDF:** Need final version for automation

---

## References

- Design System: `SPEC.md`
- Implementation Plan: `IMPLEMENTATION.md`
- Developer Environment: `.claude/README.md`

---

**Ready for engineer to begin implementation.**
