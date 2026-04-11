# neoMeta Courses Planning

**Status:** Planning
**Reference:** Jeffrey Emanuel's agent-flywheel.com courses structure
**Screenshots:** `reference-screenshots/courses/`

---

## Design Inspiration: Jeffrey Emanuel's Learning Hub

Jeffrey Emanuel uses a **Learning Hub** model with:
1. **Course overview page** - Shows all lessons with progress tracking
2. **Individual lesson pages** - Clear structure with goals, content sections, pro tips
3. **Progress system** - X/23 lessons complete, percentage, "Mark Complete" buttons
4. **Quick reference section** - Searchable reference cards
5. **Interactive wizard** - Step-by-step guided setup

**Key UI Elements:**
- Numbered lesson cards with estimated time (5-15 min each)
- Goal statement at top of each lesson
- Sections with icons (What You Have, Mental Model, What You'll Learn)
- Pro tip callouts
- "Ready to level up?" CTA at bottom
- Previous/Next navigation between lessons

---

## neoMeta Course Structure

### Philosophy: Lead Magnets → Intro Courses → Full Courses → Apps

Each pillar can have multiple tiers:
1. **Lead Magnet** - Free downloadable (poster, checklist, PDF)
2. **Intro Course** - Free short course (3-5 lessons)
3. **Full Course** - Paid comprehensive course
4. **App/Tool** - Software implementation

---

## Planned Courses

### 1. Unsettle Back (Body Compass - EAT)

**Position:** First level of Body Compass / Entry to neoMeta Health

**Lead Magnet:**
- Body Compass Poster
- "Answer Your Back" Part A PDF

**Intro Course:** "Answer Your Back" Part A
- Nutrition and movement fundamentals
- Understanding body signals
- The Three Zones (Red/Yellow/Green)
- Self-assessment framework

**Full Course:** "Answer Your Back" Part B (Video Course)
- Complete dietary transformation protocol
- Movement practices for healing
- Long-term maintenance strategies

**App:** Body Compass App

---

### 2. Freedom of Movement / Body Compass (MOVE)

**Lead Magnet:**
- Body Compass Poster
- Movement assessment checklist

**Intro Course:** "Your Body Compass" Introduction
- Understanding your body's feedback
- Basic movement principles
- Zone identification

**Full Course:** "Freedom of Movement"
- Complete movement system
- Progressive protocols
- Integration with eating and meditation

**App:** Body Compass App (movement features)

---

### 3. Art of Stillness (MEDITATE)

**Lead Magnet:**
- Meditation basics guide
- Vipassana preparation checklist

**Intro Course:** "First Steps to Stillness"
- Meditation fundamentals
- Why stillness matters
- Daily practice structure

**Full Course:** "The Art of Stillness"
- Complete meditation curriculum
- Vipassana preparation
- Advanced practices
- Integration with health stack

---

### 4. Time Preference / Integrity / Alignment / Responsibility

**Position:** Foundational course - The WHY behind neoMeta

**Lead Magnet:**
- Time Preference Assessment
- 12-Pillar Overview PDF

**Intro Course:** "Understanding Time Preference"
- What is time preference?
- The pattern beneath all good choices
- Self-assessment tools

**Full Course Options:**

#### Option A: Single "Foundations" Course
Covers all: Time Preference, Integrity, Alignment, Responsibility

#### Option B: Separate Courses per Pillar
- "Time Preference Mastery"
- "Living with Integrity"
- "Personal Alignment"
- "Radical Responsibility"

---

## Course Packaging Ideas

### Starter Pack
- Body Compass Poster (lead magnet)
- Answer Your Back Part A (intro)
- Body Compass App access

### Health Transformation Bundle
- All Health domain courses
- Body Compass Book
- App premium access

### Complete neoMeta System
- All 12 pillar courses
- All apps
- Community access
- Direct support

---

## Website Integration

### Course Listing Page (`/courses` or `/learn`)
- Grid of course cards
- Filter by domain (Health/Wealth/Connection/Foundations)
- Filter by tier (Free/Intro/Full)
- Progress tracking (if logged in)

### Individual Course Page
- Hero with course title and description
- Lesson list with time estimates
- Preview first lesson free
- CTA to enroll/purchase

### Lesson Page
- Clean reading experience
- Video embeds where applicable
- Progress tracking
- Previous/Next navigation

---

## Technical Considerations

### MVP Approach (Launch)
- Static course landing pages
- External hosting (Teachable, Thinkific, or custom)
- Link to external platform for actual course delivery

### Future Approach
- Built-in course system
- User accounts and progress tracking
- Payment integration
- Community features

---

## Decisions Made (2026-01-16)

| Question | Decision |
|----------|----------|
| **Course hosting** | Custom TypeScript system built into website |
| **Launch timing** | Placeholder pages, launch ASAP |
| **Content priority** | Unsit Your Back first (Notion content ready) |
| **Newsletter** | Substack |
| **Stats section** | Yes, live-updating (YouTube, X, app users) |

---

## Open Source LMS Options for Custom Build

### Best Fit: Next.js 15 LMS Starter

**[sonnysangha/lms-course-platform-saas-nextjs15](https://github.com/sonnysangha/lms-course-platform-saas-nextjs15-sanity-stripe-clerk-shadcn-typescript)**
- **Stack:** Next.js 15, TypeScript, Tailwind, shadcn/ui, Sanity CMS, Clerk auth, Stripe
- **License:** MIT
- **Features:** Course modules, lesson completion tracking, progress visualization, video players, mobile-friendly
- **Fit:** Very close to our stack. Could adapt or learn from architecture.

### Other Options

| Project | Stack | Notes |
|---------|-------|-------|
| [Awais512/LMS](https://github.com/Awais512/LMS) | Next.js 13, Tailwind, shadcn, TS | Simpler, good reference |
| [MrHacker26/next-lms](https://github.com/MrHacker26/next-lms) | Next.js | Lightweight |
| [shounoop/learning-management-system](https://github.com/shounoop/learning-management-system) | Next.js, Prisma, MySQL | Full features |
| [Frappe Learning](https://github.com/frappe/lms) | Python/Frappe | Not TypeScript but 100% open source |

### Larger Systems (Reference Only)
- [Open edX](https://openedx.org/) - Enterprise MOOC platform
- Moodle - PHP, very mature but heavy

---

## Content Sources

### 1. Unsit Your Back (Priority)
- **Source:** Notion database (full course content ready)
- **Action:** Connect to Notion API, extract content
- **Status:** Content complete, needs extraction

### 2. Body Compass Course
- **Source:** 11 YouTube episodes/book chapters
- **Action:** Convert each chapter to a lesson
- **Format:** Video + text summary per lesson
- **Status:** Content exists, needs reformatting

### 3. Freedom of Movement
- **Source:** 12-week course slides (one-on-one delivery)
- **Action:** Convert slides to lessons
- **Status:** Slides ready, needs digital conversion

### 4. Art of Stillness
- **Status:** Placeholder only for now

### 5. Time Preference / Foundations
- **Status:** Placeholder only for now

---

## Tech Implementation Plan

### Phase 1: Placeholders (Launch)
- Static course listing page
- Course cards with "Coming Soon" badges
- Email capture for launch notification

### Phase 2: First Course (Unsit Your Back)
- Notion API integration for content
- Basic lesson viewer
- Progress tracking (localStorage or auth)

### Phase 3: Full System
- User authentication
- Payment integration (Stripe)
- Full progress tracking
- Certificate generation

---

## Stats Section (Live Updating)

Stats to display:
- YouTube subscribers
- YouTube total views
- X (Twitter) followers
- Newsletter subscribers (Substack)
- App users (Body Compass)

**Implementation:** API calls to fetch live data, cache with reasonable refresh interval.

---

## Next Steps

1. [x] Decide on course platform approach
2. [ ] Research Notion API for Unsit Your Back extraction
3. [ ] Create course placeholder pages for website
4. [ ] Evaluate sonnysangha LMS starter for code patterns
5. [ ] Plan stats API integration

---

## Reference Screenshots

- `jeffrey-learn-hub-overview.png` - Full course listing page
- `jeffrey-lesson-page.png` - Individual lesson layout
- `jeffrey-tldr-overview.png` - Product/tool overview page style

---

**Last Updated:** 2026-01-16
