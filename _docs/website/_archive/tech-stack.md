# Website Tech Stack Recommendation

**Version:** 1.0
**Last Updated:** 2026-01-19
**Status:** Recommended - Ready for Developer

---

## Summary

| Layer | Choice | Rationale |
|-------|--------|-----------|
| **Framework** | Astro | Content-first, zero JS by default, fast |
| **Styling** | Tailwind CSS | Rapid development, design system tokens |
| **Hosting** | Vercel | Free tier, fast CDN, easy deploys |
| **Newsletter** | Substack | Via SubstackAPI.com custom embed |
| **Blog** | Substack (MVP) | No separate blog for now. See Blog Strategy. |
| **Analytics** | Umami (self-hosted) | Free on Vercel + Vercel Postgres |
| **Domain** | craigvan.com | Already owned |
| **Courses (Future)** | Next.js LMS on subdomain | See Architecture section |

---

## Architecture: Marketing + Courses

### Subdomain Split

```
craigvan.com             → Astro (marketing, philosophy, newsletter)
analytics.craigvan.com   → Umami (self-hosted analytics)
courses.craigvan.com     → Next.js LMS (auth, courses, payments) [Future]
app.craigvan.com         → Body Compass app (already Next.js)
```

### Why Split?

| Concern | Solution |
|---------|----------|
| Marketing pages need speed | Astro: 0 KB JS, instant loads |
| Courses need auth/payments | Next.js LMS: full dynamic capability |
| Unified user accounts | Clerk auth works across subdomains |
| Independent scaling | Deploy/update each separately |

### Shared Authentication (Clerk)

Clerk supports multi-domain SSO:
- User logs into courses.craigvan.com
- Same session works on craigvan.com (if needed)
- Same account as Body Compass app
- Module access control per user

### Course Platform Reference

**Open Source Starter:** `sonnysangha/lms-course-platform-saas-nextjs15`
- Stack: Next.js 15, TypeScript, Clerk, Stripe, Sanity CMS
- Features: Course modules, progress tracking, video players
- License: MIT

### MVP vs Future

| Phase | Scope |
|-------|-------|
| **MVP (Now)** | Astro marketing site only |
| **V2** | Add courses.craigvan.com with LMS |
| **V3** | Community features (Circle or custom) |

---

## Design Evolution

**Astro does NOT limit design complexity.**

| What Astro Controls | What You Control |
|---------------------|------------------|
| JavaScript delivery (minimal) | All visual design |
| Build/render strategy | CSS, animations, layouts |
| Performance optimization | Fonts, colors, interactions |

You can evolve from MVP to visually stunning site without framework changes. Astro supports:
- Complex CSS animations
- Framer Motion / GSAP (as islands)
- 3D graphics (Three.js as islands)
- Page transitions (View Transitions API)
- Any design system complexity

---

## Framework: Astro

### Why Astro over Next.js

| Factor | Astro | Next.js |
|--------|-------|---------|
| JS shipped | 0 KB default | 87 KB minimum |
| Learning curve | Lower (HTML/CSS/JS) | Higher (React required) |
| Use case fit | Content sites, portfolios | Dynamic apps |
| Build speed | Fast | Slower |
| Complexity | Simple | More complex |

**Astro is the clear winner** for a content-driven personal site with:
- Static pages (home, philosophy, systems)
- Newsletter signup form
- Minimal interactivity needed

### Astro Features We'll Use

- **Content Collections** - For blog/essays if added later
- **View Transitions** - Smooth page navigation
- **Image Optimization** - Automatic image processing
- **Islands Architecture** - Interactive components only where needed

### Version

Use Astro 5.x (latest stable as of 2026)

---

## Styling: Tailwind CSS

### Why Tailwind

- Rapid prototyping from design system
- Easy to implement design tokens (colors, spacing, typography)
- Great with Astro (official integration)
- Purges unused CSS for tiny bundles

### Configuration

```js
// tailwind.config.js
export default {
  theme: {
    extend: {
      colors: {
        'bg': '#1a1a1a',
        'bg-elevated': '#242424',
        'text': '#F5F0E6',
        'text-muted': '#A8A8A8',
        'accent': '#01A878',
        'accent-dim': '#016B4F',
      },
      fontFamily: {
        'heading': ['Space Grotesk', 'Inter', 'system-ui', 'sans-serif'],
        'body': ['Inter', 'SF Pro Text', 'system-ui', 'sans-serif'],
        'mono': ['JetBrains Mono', 'Fira Code', 'monospace'],
      },
      // ... spacing, typography scale from design-system.md
    }
  }
}
```

---

## Hosting: Vercel or Cloudflare Pages

### Option A: Vercel (Recommended)

**Pros:**
- Best-in-class DX
- Automatic deploys from GitHub
- Built-in analytics
- Edge functions if needed later
- Generous free tier

**Setup:**
1. Connect GitHub repo
2. Auto-detects Astro
3. Deploy

### Option B: Cloudflare Pages

**Pros:**
- Unlimited bandwidth (free tier)
- Global CDN
- No cold starts
- Cloudflare ecosystem (DNS, security)

**Choose Cloudflare if:** Already using Cloudflare for DNS or want unlimited bandwidth.

### Domain Setup

1. Point `craigvan.com` to hosting provider
2. Enable HTTPS (automatic)
3. Set up `www` redirect

---

## Newsletter: Substack

### Integration: SubstackAPI.com (Custom Embed)

**Chosen approach:** [SubstackAPI.com](https://substackapi.com/) for fully custom-styled signup forms.

**Why this over alternatives:**
| Option | Verdict |
|--------|---------|
| Link to Substack | User leaves site - breaks immersion |
| Official iframe embed | Substack branding, limited styling |
| **SubstackAPI.com** | **Full design control, stays on site** |
| Direct Substack API | Undocumented, could break |

### Implementation

**Step 1:** Go to [substackapi.com](https://substackapi.com/)

**Step 2:** Enter publication name, customize colors to match design system:
- Background: `#1a1a1a`
- Text: `#F5F0E6`
- Button: `#01A878`
- Button text: `#1a1a1a`

**Step 3:** Copy generated code into Astro component:

```astro
<!-- src/components/NewsletterForm.astro -->
<form class="newsletter-form">
  <input
    type="email"
    name="email"
    placeholder="Enter your email"
    class="input"
    required
  />
  <button type="submit" class="btn-primary">Join</button>
</form>
<script src="https://substackapi.com/widget.js"
        data-publication="craigvan"
        async></script>
```

**Step 4:** Style with Tailwind to match blueprint aesthetic.

### User Flow
1. User enters email on craigvan.com
2. Form submits to Substack via SubstackAPI
3. User receives confirmation email from Substack
4. User confirms → added to your list

### Notes
- Substack requires double opt-in (confirmation email)
- Subscriber management in Substack dashboard
- No Substack branding visible on your site
- Free to use

---

## Blog Strategy

### Decision: Substack as Primary (MVP)

**For MVP:** No separate blog on craigvan.com. Substack serves as both newsletter AND blog.

| Phase | Strategy |
|-------|----------|
| **MVP (Now)** | Substack only. Link to archive from site. |
| **V2** | Add `/writing` page displaying Substack RSS feed |
| **V3** | Differentiated: evergreen guides on site, essays on Substack |

### Rationale

| Factor | Decision |
|--------|----------|
| Duplicate workflow | Avoid - focus on creating, not republishing |
| SEO ownership | Accept trade-off for now - YouTube drives discovery |
| Platform risk | Acceptable - Substack content exportable |
| Maintenance | Minimize - one publishing location |

### What This Means for craigvan.com

**MVP site will NOT have:**
- `/blog` or `/writing` section with full articles
- Self-hosted content management

**MVP site WILL have:**
- Newsletter signup (SubstackAPI embed)
- Link to Substack archive ("Read my writing →")
- Philosophy/Systems pages (static, not blog posts)

### Future Evolution (V3)

When ready to own SEO:
```
craigvan.com/guides     → Evergreen pillar content (12 pillars, frameworks)
Substack                → Weekly essays, personal updates, experiments
```

Different content for each platform = no duplicate concerns, full SEO benefit.

### If Republishing Later

Best practice to avoid duplicate content penalties:
1. Publish on own blog FIRST
2. Wait 1-2 days for Google indexing
3. Post to Substack with modifications:
   - Different title
   - Tweaked intro/outro
   - Platform-specific CTAs

---

## Analytics: Umami (Self-Hosted)

### Why Umami

| Factor | Umami |
|--------|-------|
| Cost | Free (Vercel + Vercel Postgres) |
| Privacy | GDPR compliant, no cookies |
| Script size | < 1KB |
| Complexity | Low |
| Data ownership | Full |

### Deployment

**Stack:** Umami on Vercel + Vercel Postgres (free tier)

**Setup steps:**
1. Fork [umami-software/umami](https://github.com/umami-software/umami)
2. Create Vercel Postgres database
3. Deploy to Vercel with environment variables
4. Add custom domain: `analytics.craigvan.com`
5. Add tracking script to Astro site

### Tracking Code

```html
<!-- Add to BaseLayout.astro <head> -->
<script
  defer
  src="https://analytics.craigvan.com/script.js"
  data-website-id="YOUR_WEBSITE_ID"
></script>
```

### What You Get

- Page views, unique visitors
- Referrer sources
- Device/browser/country breakdown
- Real-time dashboard
- No cookie banner required

---

## MVP Scope

### Pages for V1

| Page | Route | Content Source |
|------|-------|----------------|
| Home | `/` | `pages/home.md` |
| Philosophy | `/philosophy` | `pages/philosophy.md` |
| Systems | `/systems` | `pages/systems.md` |
| Poster LP | `/poster` | `content/body-compass-poster/` |

### Components for V1

| Component | Required |
|-----------|----------|
| Header/Nav | ✅ |
| Hero + Newsletter | ✅ |
| Pillar Cards (EAT/MOVE/MEDITATE) | ✅ |
| Footer | ✅ |
| Philosophy content sections | ✅ |
| Systems content sections | ✅ |
| Poster landing page | ✅ |

---

## Contact & Social

| Platform | Handle/URL |
|----------|------------|
| X (Twitter) | [@craigvandotcom](https://x.com/craigvandotcom) |
| YouTube | [@craigvandotcom](https://youtube.com/@craigvandotcom) |
| GitHub | [@craigvandotcom](https://github.com/craigvandotcom) |
| Substack | [craigvandotcom.substack.com](https://craigvandotcom.substack.com) |
| Email | mail@craigvan.com |

---

## Pre-Build Checklist

### Assets Needed (From Craig)

- [x] **Logo** - PNG format, light version (`assets/logo/cv-logo-light.png`)
- [x] **Favicon** - All sizes generated (`assets/favicon/`)
- [x] **Pillar icons** - PNG for EAT, MOVE, MEDITATE (`assets/icons/pillar-*-styled.png`)
- [ ] **Profile photo** (optional) - For about section if needed

### Setup Tasks (Developer)

- [ ] Create Vercel account / connect to GitHub
- [ ] Initialize Astro project with Tailwind
- [ ] Configure design tokens from `design-system.md`
- [ ] Set up Vercel Postgres + Umami
- [ ] Configure SubstackAPI.com embed
- [ ] Add analytics tracking script

### Content Ready

- [x] Home page copy (`pages/home.md`)
- [x] Philosophy content (`pages/philosophy.md`)
- [x] Systems content (`pages/systems.md`)
- [x] Design system (`design-system.md`)
- [x] Mockups (`output/website-mvp/`)
- [ ] Poster landing page copy (verify `content/body-compass-poster/`)

### SEO Metadata

| Page | Title | Description |
|------|-------|-------------|
| Home | Craig van Heerden - Building Life Operating Systems | eat. move. meditate. A framework for human sovereignty. |
| Philosophy | Philosophy - Craig van Heerden | The WHY behind neoMeta: time preference, AI alignment, integrity. |
| Systems | Systems - Craig van Heerden | The complete neoMeta framework: 12 pillars across Health, Wealth, Connection. |
| Poster | Body Compass - Free Poster | Your visual guide to nutrition sovereignty. Download the free Body Compass poster. |

### Post-Launch

- [ ] Verify analytics tracking
- [ ] Test newsletter signup flow
- [ ] Submit sitemap to Google Search Console
- [ ] Test on mobile devices
- [ ] Share on social channels

---

## Project Structure

```
software/website/
├── src/
│   ├── components/
│   │   ├── Header.astro
│   │   ├── Footer.astro
│   │   ├── PillarCard.astro
│   │   ├── NewsletterForm.astro
│   │   └── Badge.astro
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   ├── index.astro          # Home
│   │   ├── philosophy.astro     # Philosophy page
│   │   ├── systems.astro        # 12 Pillars
│   │   ├── poster.astro         # Lead magnet
│   │   └── [...slug].astro      # Dynamic pages (future)
│   ├── styles/
│   │   └── global.css
│   └── content/                  # Future blog/essays
├── public/
│   ├── fonts/
│   ├── icons/
│   └── images/
├── astro.config.mjs
├── tailwind.config.js
├── package.json
└── .claude/                      # Developer agent skills (later)
```

---

## MVP Scope

### Pages for V1

| Page | Route | Content Ready |
|------|-------|---------------|
| Home | `/` | Yes (pages/home.md) |
| Philosophy | `/philosophy` | Yes (pages/philosophy.md) |
| Systems | `/systems` | Yes (pages/systems.md) |
| Poster LP | `/poster` | Yes (body-compass-poster/) |

### Components for V1

| Component | Priority |
|-----------|----------|
| Header/Nav | Required |
| Hero | Required |
| PillarCard | Required |
| NewsletterForm | Required |
| Footer | Required |
| Badge | Required |

### Deferred to V2

- Blog/Essays section
- About page
- Contact page
- Apps page
- Search functionality
- Comments/Discussion

---

## Developer Handoff Checklist

### Documents Ready

- [x] `design-system.md` - All visual specs
- [x] `tech-stack.md` - This document
- [x] `pages/home.md` - Home page content
- [x] `pages/philosophy.md` - Philosophy content
- [x] `pages/systems.md` - Systems content
- [ ] Mockup images - Reference visuals

### Assets Needed

- [ ] Space Grotesk + Inter fonts (or Google Fonts links)
- [ ] Pillar icons (EAT, MOVE, MEDITATE) - SVG
- [ ] Social icons - SVG
- [ ] Logo/favicon
- [ ] Substack publication created
- [ ] SubstackAPI.com embed code configured

### Developer Setup

When ready to begin:
1. Create `software/website/` as Astro project
2. Set up `.claude/skills/` for developer agent
3. Architect subagent handles skill setup

---

## Implementation Order

1. **Setup** - Initialize Astro + Tailwind, configure design tokens
2. **Layout** - BaseLayout with Header/Footer
3. **Home** - Hero section + Pillar cards
4. **Newsletter** - Embed Beehiiv form
5. **Philosophy** - Long-form content page
6. **Systems** - 12 pillars page
7. **Poster LP** - Lead magnet page
8. **Deploy** - Connect to Vercel/Cloudflare
9. **Domain** - Point craigvan.com

---

## Sources

Research conducted 2026-01-19:

- [Astro vs NextJS 2026 Comparison](https://www.aalpha.net/blog/astro-vs-nextjs-comparison/)
- [Top Static Site Generators 2025](https://cloudcannon.com/blog/the-top-five-static-site-generators-for-2025-and-when-to-use-them/)
- [Astro vs Next.js: Why I Prefer Astro](https://www.nray.dev/blog/astro-vs-nextjs-why-i-prefer-astro-for-static-sites/)
- [Beehiiv Embedded Subscribe Forms](https://www.beehiiv.com/support/article/12977090590487-creating-and-embedding-beehiiv-subscribe-forms)
- [Substack Embed Documentation](https://support.substack.com/hc/en-us/articles/360041759232-Can-I-embed-a-signup-form-for-my-Substack-publication)

---

**Next Step:** When ready to build, architect subagent sets up developer skills in `.claude/` directory.
