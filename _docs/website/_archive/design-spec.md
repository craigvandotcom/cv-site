# neoMeta Website Design Specification

**For:** Developer Handoff
**Status:** Ready for Build
**Reference:** Jeffrey Emanuel's site (jeffreyemanuel.com)
**Brand Guide:** `/infrastructure/tools/src/image-gen/BRAND.md`

---

## V1 Strategy: Health Operating System

**Decision:** V1 = Health Operating System. Niche focus for market entry, expand to full Life OS after proven.

**Rationale:**
- Clean market positioning (niche = credibility)
- Body Compass app is LIVE (tangible proof)
- Established YouTube content in Health domain
- Complete offer stack ready for 3 pillars
- Hormozi-inspired: solve all problems in one journey, not sell components

**Approach:**
- Hero: "Building a **Health** Operating System"
- Subheadline: "Master your body. The rest follows."
- 3 pillar journeys: Nutrition / Movement / Stillness
- Each pillar = complete offer (app + course + community)
- No separate Apps/Courses pages—journeys contain both
- Philosophy page shows full Life OS vision (future expansion)

**Key Insight:** Depth over breadth. Master one domain completely before expanding.

---

## Product Structure (V1)

### Pillar Offers

| Pillar | Journey Page | App | Course(s) | Status |
|--------|-------------|-----|-----------|--------|
| **Eat** | `/nutrition` | Body Compass | Body Compass Course | App LIVE, Course ready |
| **Move** | `/movement` | Freedom of Movement | Un-sit Your Back, Freedom of Movement | Courses ready, App coming |
| **Meditate** | `/stillness` | Art of Stillness | Art of Stillness | Both coming |

### Journey Flow (Each Pillar)
1. Land on pillar page → understand the transformation
2. Download app (or get notified when ready)
3. Get poster (physical anchor)
4. Join newsletter (ongoing value)
5. Complete course lessons (deepen understanding)
6. Join community (accountability + referrals)

**Design Principle:** One integrated flow per pillar. Not "buy course" + "download app" separately—selling transformation, not components.

---

## Design System

### Color Palette

| Name | Hex | CSS Variable | Usage |
|------|-----|--------------|-------|
| **Deep Charcoal** | `#1a1a1a` | `--bg-primary` | Primary background |
| **Warm Cream** | `#F5F0E6` | `--text-primary` | Primary text, light elements |
| **Brand Green** | `#01A878` | `--accent` | Accent, CTAs, glow effects |
| **Deep Charcoal Green** | `#0D3D3D` | `--bg-secondary` | Background undertones, depth |
| **Bitcoin Orange** | `#F7931A` | `--accent-wealth` | Wealth/Money topics only |

### Zone Colors (Body Compass Only)

| Zone | Hex | Usage |
|------|-----|-------|
| Red Zone | `#D62828` | Avoid, stop |
| Yellow Zone | `#FCBF49` | Test, caution |
| Green Zone | `#2D6A4F` | Enjoy, amplify |

---

## Typography

### Fonts

| Usage | Font Family | Weight | Fallback |
|-------|-------------|--------|----------|
| Headlines | Space Grotesk | 700 (Bold) | system-ui |
| Body | Inter | 400, 500, 600 | system-ui |
| Code/System | JetBrains Mono | 400 | monospace |

### Scale (rem)

| Level | Size | Line Height | Usage |
|-------|------|-------------|-------|
| h1 | 3.5rem (56px) | 1.1 | Hero headlines |
| h2 | 2.5rem (40px) | 1.2 | Section titles |
| h3 | 1.75rem (28px) | 1.3 | Card titles |
| h4 | 1.25rem (20px) | 1.4 | Subsections |
| body | 1rem (16px) | 1.6 | Paragraph text |
| small | 0.875rem (14px) | 1.5 | Captions, labels |

---

## Component Patterns

### Cards

```css
/* Base card style */
.card {
  background: rgba(26, 26, 26, 0.8);
  border: 1px solid rgba(1, 168, 120, 0.2);
  border-radius: 16px;
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.card:hover {
  border-color: rgba(1, 168, 120, 0.5);
  box-shadow: 0 0 30px rgba(1, 168, 120, 0.15);
  transform: translateY(-2px);
}
```

### Buttons

| Type | Style |
|------|-------|
| Primary | Solid green (#01A878), white text |
| Secondary | Green border, transparent bg, green text |
| Ghost | Transparent, cream text, hover: subtle bg |

### Badges

| Status | Style |
|--------|-------|
| LIVE | Solid green background |
| COMING [YEAR] | Green outline |
| BUILDING | Blue background |
| SHARING | Purple background |
| PLACEHOLDER | Gray dotted outline |

---

## Layout System

### Breakpoints

| Name | Width | Usage |
|------|-------|-------|
| mobile | < 640px | Single column |
| tablet | 640-1024px | 2-column grids |
| desktop | 1024-1440px | Full layout |
| wide | > 1440px | Max-width container |

### Container

```css
.container {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 24px;
}

@media (min-width: 1440px) {
  .container {
    max-width: 1400px;
  }
}
```

### Grid

- 12-column grid for complex layouts
- CSS Grid for section layouts
- Flexbox for component layouts
- Gap: 24px (desktop), 16px (mobile)

---

## Animations (Framer Motion)

### Page Transitions

```typescript
const pageVariants = {
  initial: { opacity: 0, y: 20 },
  enter: { opacity: 1, y: 0, transition: { duration: 0.4 } },
  exit: { opacity: 0, y: -20, transition: { duration: 0.3 } }
};
```

### Section Reveals

```typescript
const sectionVariants = {
  hidden: { opacity: 0, y: 40 },
  visible: {
    opacity: 1,
    y: 0,
    transition: { duration: 0.6, ease: "easeOut" }
  }
};
```

### Hover Effects

- Cards: translateY(-2px), enhanced border glow
- Buttons: scale(1.02), brightness increase
- Links: color transition to accent
- Timeline items: glow effect on dot

### Interactive Elements

- 12-pillar visualization: Orbital animation (if interactive)
- Stats counters: Count-up animation on scroll into view
- Profile image: Subtle hover scale

---

## Navigation

### Desktop

- Fixed header, dark background
- Logo left, menu center/right
- Primary CTA button right
- Blur background on scroll

### Mobile

- Hamburger menu
- Full-screen overlay
- Staggered menu item reveal

### Menu Items (V1)

Home | About | Nutrition | Movement | Stillness | Writing | Contact

*Note: Each pillar page is a complete journey (app + course + community). No separate Apps/Courses pages.*

---

## Page Sections (Home) - Health Operating System

### 1. Hero
- Full viewport height
- Headline: "Building a **Health** Operating System"
- Subheadline: "Master your body. The rest follows."
- Visual: 3 pillars diagram (Eat/Move/Meditate) or human silhouette with systems
- Primary CTA: "Start Your Journey" → leads to pillar selection
- Secondary CTA: Newsletter
- Subtle green glow effects

### 2. The Three Pillars
- 3 large pillar cards: Nutrition / Movement / Stillness
- Each card: icon, pillar name, one-line promise, "Begin →" CTA
- Visually prominent—this IS the product
- Each links to respective journey page

### 3. Philosophy (WHY)
- Brief intro to systems thinking for health
- "Why an operating system?" explanation
- Link to full Philosophy page (shows full Life OS vision)

### 4. Featured Journey: Nutrition (Body Compass)
- Hero product spotlight
- App screenshots
- "Download the app, get the poster, start the course"
- LIVE badge prominent
- Social proof (users, testimonials if available)

### 5. Statistics
- 4 stat cards in row
- YouTube subscribers, Body Compass users, course completions
- Animated count-up
- Green accent numbers

### 6. Building in Public
- Journey narrative
- "Follow as we build the complete Health OS"
- YouTube content samples
- "Follow the build" CTA

### 7. Writing
- Featured health essays
- Link to full Writing page

### 8. Newsletter CTA
- "Join the Health OS community"
- Substack integration

### 9. Footer
- Social links
- Navigation
- Copyright

*V2 additions: Movement & Stillness journeys complete, Timeline, Testimonials, Life OS expansion*

---

## Pillar Journey Pages

Each pillar (Nutrition, Movement, Stillness) has a dedicated journey page with consistent structure:

### Journey Page Structure

#### 1. Hero
- Pillar name + transformation promise
- "Transform your [nutrition/movement/stillness]"
- Visual representing the pillar

#### 2. The Problem
- Pain points this pillar solves
- Relatable frustrations
- "You've tried everything..."

#### 3. The Solution (The Journey)
- Overview of the complete offer
- App + Course + Poster + Community
- "Everything you need, nothing you don't"

#### 4. Step 1: Download the App
- App screenshots and features
- Download CTAs (iOS/Android or waitlist)
- "Start tracking today"

#### 5. Step 2: Get the Poster
- Physical anchor for the system
- Visual of the poster
- Free download or order printed

#### 6. Step 3: Complete the Course
- Course overview and curriculum
- Lesson previews
- "Deepen your understanding"

#### 7. Step 4: Join the Community
- Community benefits
- Accountability, support, Q&A
- Discord/Circle link

#### 8. Pricing/CTA
- Clear pricing (if applicable)
- "Begin Your Journey" button
- Money-back guarantee if relevant

#### 9. FAQ
- Common questions for this pillar
- Objection handling

### Pillar-Specific Content

**Nutrition (`/nutrition`)**
- App: Body Compass (LIVE)
- Course: Body Compass Course
- Poster: Food zones visual
- Promise: "Build an intuitive relationship with food"

**Movement (`/movement`)**
- App: Freedom of Movement (coming)
- Courses: Un-sit Your Back, Freedom of Movement
- Poster: Movement protocol visual
- Promise: "Reclaim your body's natural movement"

**Stillness (`/stillness`)**
- App: Art of Stillness (coming)
- Course: Art of Stillness
- Poster: Meditation protocol visual
- Promise: "Cultivate presence and mental clarity"

---

## Interactive Elements

### 12-Pillar Visualization

**Options:**

**A. Full Interactive (More Dev Work)**
- Three nested animated rings
- Click/hover to explore domains
- Orbital motion animation
- Pulsing indicators

**B. Static with Hover (Recommended MVP)**
- SVG diagram
- Hover states on domains
- Links to relevant pages
- No complex animation

**Decision Needed:** Interactive or static?

### Stats Section

- Pull live data from APIs
- YouTube Data API (subscribers, views)
- Twitter/X API (followers)
- Substack (manual or scrape)
- Body Compass (internal API)
- Cache with 1-hour refresh

---

## Asset Requirements

### Images Needed

| Asset | Size | Status |
|-------|------|--------|
| Brand headshot | 800x800+ | Ready (Craig to share) |
| Body Compass screenshots | 1080x1920 | Needed |
| Book cover mockup | TBD | Future |
| 12-pillar diagram | SVG | To create |
| Domain icons (9) | SVG | To create |
| Foundation icons (3) | SVG | To create |

### Reference Screenshots

Located: `/software/website/reference-screenshots/`
- Home page sections (14 screenshots)
- Course layouts (3 screenshots)

---

## Technical Implementation

### Stack

| Layer | Choice |
|-------|--------|
| Framework | Next.js 15 (App Router) |
| Styling | Tailwind CSS |
| Animations | Framer Motion |
| UI Components | shadcn/ui (customized) |
| Newsletter | Substack embed |
| Analytics | Umami (self-hosted) |
| Hosting | Vercel |

### Key Dependencies

```json
{
  "next": "^15.0.0",
  "tailwindcss": "^3.4.0",
  "framer-motion": "^11.0.0",
  "@radix-ui/react-*": "latest",
  "lucide-react": "latest"
}
```

### Folder Structure

```
/app
  /page.tsx              # Home
  /about/page.tsx
  /philosophy/page.tsx
  /nutrition/page.tsx    # Pillar journey
  /movement/page.tsx     # Pillar journey
  /stillness/page.tsx    # Pillar journey
  /writing/page.tsx
  /contact/page.tsx
  /blog/[slug]/page.tsx
/components
  /ui                    # shadcn components
  /sections              # Page sections
  /navigation            # Nav components
  /journey               # Pillar journey components
  /visualizations        # Stats, diagrams
/lib
  /utils.ts
  /api.ts                # Stats fetching
/content
  /blog                  # MDX files
  /courses               # Course content (MDX)
/public
  /images
  /icons
  /posters               # Downloadable PDFs
```

---

## Developer Checklist

### Before Build
- [ ] Receive brand headshot from Craig
- [ ] Set up Umami analytics instance
- [ ] Get Substack embed code
- [ ] Body Compass app screenshots
- [ ] Body Compass poster PDF
- [ ] Course content ready (Body Compass)

### V1: Health OS Launch
- [ ] Navigation + footer
- [ ] Home page (Health OS sections)
- [ ] About page
- [ ] Philosophy page (includes Life OS vision)
- [ ] Nutrition journey page (Body Compass - complete)
- [ ] Movement journey page (placeholder/coming soon)
- [ ] Stillness journey page (placeholder/coming soon)
- [ ] Writing page (health essays)
- [ ] Contact page with form
- [ ] Newsletter signup working
- [ ] Responsive design
- [ ] Basic animations

### V2: Complete Health OS
- [ ] Movement journey page (full - Un-sit + Freedom of Movement)
- [ ] Stillness journey page (full - Art of Stillness)
- [ ] Course system integration
- [ ] Community integration (Discord/Circle)
- [ ] Live stats integration
- [ ] Blog system (MDX)
- [ ] Enhanced animations

### V3: Life OS Expansion
- [ ] Wealth domain journeys
- [ ] Connection domain journeys
- [ ] Full 12-pillar interactive visualization
- [ ] SEO optimization
- [ ] Performance optimization

---

## Open Decisions

1. ~~**12-Pillar Visualization:** Interactive animated or static with hover?~~ → Static with hover for V1

---

## Decisions Made

| Decision | Choice | Rationale |
|----------|--------|-----------|
| V1 Scope | Health Operating System | Niche focus, depth over breadth, prove model first |
| Site Structure | Pillar journeys, not Apps/Courses pages | Hormozi-style: sell transformation, not components |
| Hero Headline | "Health Operating System" | Clear positioning, expand to Life OS later |
| Navigation | Home, About, Nutrition, Movement, Stillness, Writing, Contact | Pillars as primary nav, not products |
| Journey Flow | App → Poster → Course → Community | Integrated offer stack per pillar |

---

**Last Updated:** 2026-01-16
