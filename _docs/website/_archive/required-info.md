# Required Information for craigvan.com

**Domain:** craigvan.com (confirmed)
**Primary CTA:** Newsletter signup (confirmed)
**Color Scheme:** Brand system from image-gen templates (confirmed)
**Build Approach:** Craig builds, I design (confirmed)
**Scope:** Full website (confirmed)

---

## DECISIONS MADE

| Item | Decision |
|------|----------|
| Domain | craigvan.com |
| Primary CTA | Newsletter signup |
| Colors | Brand green (#01A878), Deep charcoal (#1a1a1a), Warm cream (#F5F0E6) |
| Style | Dark theme like Jeffrey's, blueprint aesthetic with green glow |
| Builder | Craig (I provide design spec + code) |
| Scope | Full site, all sections |

---

## STILL NEEDED FROM CRAIG

### High Priority (Blocks Design)

#### 1. Photography (CONFIRMED)
- [x] **Professional headshot** - `content/profile-picture.png`
- [ ] **Working/lifestyle shots** (optional but recommended)
  - Options: Coding, meditation posture, food prep, movement practice

#### 2. Metrics for Statistics Section
- [ ] Newsletter subscriber count (current)
- [ ] YouTube subscriber count (current)
- [ ] App users count (if comfortable sharing)
- [ ] X/Twitter follower count

#### 3. Timeline Dates (CONFIRMED)
| Period | Role/Study | Location |
|--------|-----------|----------|
| 2009-2011 | Mechanical Engineering | South Africa |
| 2012-2015 | Biomedical Sciences + Biokinetics | South Africa |
| 2016-2020 | Own therapy practice (Biokinetician) | South Africa |
| 2020-2021 | Business Administration studies | Europe |
| 2021-2023 | Data Science Masters | Europe |
| 2023-Present | Technical Support Engineer | Adyen, Amsterdam |
| Ongoing | 11 Vipassana retreats | Various |

**Still needed:**
- [ ] First Vipassana retreat year
- [ ] Body Compass project start year
- [ ] Exact Amsterdam move date

#### 4. Social Links (CONFIRMED)
**Handle across all platforms:** @craigvandotcom

| Platform | URL |
|----------|-----|
| X/Twitter | https://x.com/craigvandotcom |
| YouTube | https://youtube.com/@craigvandotcom |
| Instagram | https://instagram.com/craigvandotcom |
| GitHub | https://github.com/craigvandotcom |
| LinkedIn | https://linkedin.com/in/craigvandotcom |
| Newsletter | https://substack.com/@craigvandotcom |
| Body Compass | https://bodycompass.app |

---

### Medium Priority (Before Launch)

#### 5. Testimonials
- [ ] 2-3 Body Compass app user quotes
- [ ] Name + context for each (e.g., "Sarah, struggled with IBS for 5 years")
- [ ] If none yet, we can launch without and add later

#### 6. Featured Content
- [ ] 3-5 best essays/newsletters to feature on writing section
- [ ] 3-5 best YouTube videos to embed
- [ ] 3-5 best X threads to link

#### 7. App Assets
- [ ] Body Compass app screenshots (3-4 key screens)
- [ ] App store links (iOS/Android) if live
- [ ] Body Compass book cover (when available)

---

### Low Priority (Can Add Post-Launch)

#### 8. Additional Graphics
- [ ] Sovereignty Stack diagram (Eat-Move-Meditate visualization)
  - I can generate this with image-gen scripts
- [ ] Three Zones diagram (Red/Yellow/Green)
- [ ] neoMeta 12-pillar visual (for deeper pages)

#### 9. Extended Copy
- [ ] Full bio (300+ words for About page)
- [ ] Press/media kit content
- [ ] FAQ content

---

## TECHNICAL SETUP NEEDED

### Domain & Hosting
- [ ] Verify craigvan.com ownership
- [ ] Hosting: Vercel recommended (works with Next.js)
- [ ] DNS configuration when ready

### Integrations
- [x] Newsletter provider: **Substack** (substack.com/@craigvandotcom)
- [x] Analytics: **Umami** (free tier 100k events/month, GDPR compliant, self-hostable)
- [x] Contact form destination: **mail@craigvan.com**

---

## TECH STACK (Proposed)

| Layer | Choice | Reason |
|-------|--------|--------|
| Framework | Next.js 15 | Performance, SEO, familiar |
| Styling | Tailwind CSS | Matches Jeffrey's approach, fast iteration |
| Animations | Framer Motion | Smooth transitions, orbiting elements |
| Hosting | Vercel | Easy deploy, good DX |
| CMS | Markdown files in /blog directory | Simple, IDE-editable, no CMS overhead |
| Newsletter | Substack | Embedded form component |

**Alternative:** Could use Astro if prefer lighter weight, but Next.js matches Jeffrey's approach.

---

## DESIGN SPEC DELIVERABLES

When requirements above are gathered, I will provide:

1. **Figma/design mockup** (if helpful)
2. **Component structure** - React component breakdown
3. **Full page code** - Working Next.js + Tailwind implementation
4. **Color variables** - CSS custom properties from brand system
5. **Animation specs** - Framer Motion configurations
6. **Responsive breakpoints** - Mobile/tablet/desktop

---

## COURSES FEATURE (NEW)

**Status:** Planning phase - see `courses-planning.md` for full details

### Planned Courses

| Course | Domain | Status |
|--------|--------|--------|
| Unsettle Back / Answer Your Back | Health (Eat) | Planning |
| Freedom of Movement / Body Compass | Health (Move) | Planning |
| Art of Stillness | Health (Meditate) | Planning |
| Time Preference / Foundations | Introduction | Planning |

### Course Tiers
1. **Lead Magnets** - Free (Body Compass Poster, PDFs)
2. **Intro Courses** - Free short courses (3-5 lessons)
3. **Full Courses** - Paid comprehensive courses
4. **Apps** - Body Compass, future apps

### Key Decisions (RESOLVED 2026-01-16)
- [x] **Course platform:** Custom TypeScript build (leverage open source starters)
- [x] **Launch approach:** Placeholder pages, launch ASAP
- [x] **Content priority:** Unsit Your Back first (full Notion course), then Body Compass (11 YouTube chapters)
- [x] **Newsletter:** Substack (confirmed)
- [x] **Stats:** Yes, live-updating (YouTube, X, app users)

---

## QUESTIONS FOR CRAIG

### Resolved
- [x] **Stats section** - Yes, live-updating (YouTube, X, app users)
- [x] **Newsletter platform** - Substack
- [x] **Course platform** - Custom TypeScript build
- [x] **Course content priority** - Unsit Your Back first (Notion), then Body Compass (YouTube chapters)

### Still Open
1. **12-Pillar Visualization** - Interactive (more dev work) or static image?

### Recently Resolved (2026-01-16)
- [x] **YouTube** - Live at youtube.com/@craigvandotcom
- [x] **Body Compass** - Live at bodycompass.app
- [x] **LinkedIn** - linkedin.com/in/craigvandotcom
- [x] **Substack** - substack.com/@craigvandotcom
- [x] **Contact email** - mail@craigvan.com
- [x] **Headshot** - Has brand headshot ready to share
- [x] **Launch timing** - ASAP
- [x] **Analytics** - Umami (free tier, self-hostable)

---

## REFERENCE

- **Jeffrey Emanuel's site:** https://jeffreyemanuel.com/
- **Brand colors:** `/scripts/image-gen/BRAND.md`
- **Design templates:** `/scripts/image-gen/templates/`
- **Brainstorm document:** `/software/website/brainstorm.md`
