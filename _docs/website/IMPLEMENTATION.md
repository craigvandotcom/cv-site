# craigvan.com - Implementation Plan

**Version:** 1.0
**Last Updated:** 2026-01-19
**Status:** Ready to Execute

---

## Overview

This document provides a sequenced implementation plan for the MVP website. Each phase builds on the previous, with clear deliverables and acceptance criteria.

**Total Phases:** 5
**Estimated Effort:** Execute phases in sequence

---

## Prerequisites

Before starting, ensure:

- [ ] Node.js 20.x installed
- [ ] pnpm installed (`npm install -g pnpm`)
- [ ] Vercel CLI installed (`pnpm install -g vercel`)
- [ ] Access to Vercel account
- [ ] Access to Substack publication

---

## Phase 1: Project Setup

### Objective
Initialize Astro project with Tailwind and configure design tokens.

### Tasks

#### 1.1 Initialize Astro Project
```bash
cd software/website
pnpm create astro@latest . --template minimal --typescript strict
```

#### 1.2 Add Tailwind Integration
```bash
pnpm astro add tailwind
```

#### 1.2.1 Add Lucide Icons
```bash
pnpm add lucide-astro
```

#### 1.3 Configure Tailwind with Design Tokens

Create `tailwind.config.js`:
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
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
      spacing: {
        '18': '4.5rem',
        '22': '5.5rem',
      }
    }
  },
  plugins: [],
}
```

#### 1.4 Create Global Styles

Create `src/styles/global.css`:
```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Space+Grotesk:wght@600;700&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    font-family: 'Inter', system-ui, sans-serif;
    background-color: #1a1a1a;
    color: #F5F0E6;
  }
}

@layer utilities {
  .accent-glow {
    color: #01A878;
    text-shadow: 0 0 20px rgba(1, 168, 120, 0.6),
                 0 0 40px rgba(1, 168, 120, 0.3);
  }

  .glow-subtle {
    box-shadow: 0 0 10px rgba(1, 168, 120, 0.3);
  }

  .glow-medium {
    box-shadow: 0 0 20px rgba(1, 168, 120, 0.4);
  }
}
```

#### 1.5 Copy Assets to Public

```bash
cp -r assets/favicon/* public/favicon/
cp -r assets/icons/* public/icons/
cp -r assets/logo/* public/logo/
```

#### 1.6 Configure Astro

Update `astro.config.mjs`:
```js
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  integrations: [tailwind()],
  site: 'https://craigvan.com',
});
```

### Deliverables
- [ ] Astro project initialized
- [ ] Tailwind configured with design tokens
- [ ] Lucide icons installed
- [ ] Global styles created
- [ ] Assets in public folder
- [ ] `pnpm dev` runs without errors

### Acceptance Criteria
- Dev server starts successfully
- Tailwind classes work
- Custom colors available (bg-bg, text-accent, etc.)
- Lucide icons import correctly

---

## Phase 2: Base Layout & Components

### Objective
Create reusable layout and core components.

### Tasks

#### 2.1 Create BaseLayout

Create `src/layouts/BaseLayout.astro`:
```astro
---
interface Props {
  title: string;
  description?: string;
  ogImage?: string;
}

const {
  title,
  description = "Building Life Operating Systems. Start with health sovereignty.",
  ogImage = "/og/home.png"
} = Astro.props;

const siteUrl = "https://craigvan.com";
const canonicalUrl = new URL(Astro.url.pathname, siteUrl);
---

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <meta name="description" content={description} />

    <!-- Favicon -->
    <link rel="icon" type="image/x-icon" href="/favicon/favicon.ico" />
    <link rel="apple-touch-icon" href="/favicon/apple-touch-icon.png" />

    <!-- Open Graph -->
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:image" content={`${siteUrl}${ogImage}`} />
    <meta property="og:url" content={canonicalUrl} />
    <meta property="og:type" content="website" />

    <!-- Twitter -->
    <meta name="twitter:card" content="summary_large_image" />
    <meta name="twitter:site" content="@craigvandotcom" />
    <meta name="twitter:title" content={title} />
    <meta name="twitter:description" content={description} />
    <meta name="twitter:image" content={`${siteUrl}${ogImage}`} />

    <!-- Canonical -->
    <link rel="canonical" href={canonicalUrl} />

    <title>{title}</title>
  </head>
  <body class="bg-bg text-text min-h-screen">
    <!-- Skip to content for accessibility -->
    <a href="#main-content" class="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4 bg-accent text-bg px-4 py-2">
      Skip to content
    </a>
    <slot />
  </body>
</html>
```

#### 2.2 Create Header Component

Create `src/components/Header.astro`:
```astro
---
// Header with logo and optional CTA
---

<header class="fixed top-0 w-full bg-bg/80 backdrop-blur-sm border-b border-text/10 z-50">
  <div class="max-w-6xl mx-auto px-6 h-16 flex items-center justify-between">
    <a href="/" class="flex items-center">
      <img src="/logo/cv-logo-light.png" alt="CV" class="h-8" />
    </a>
    <nav class="flex items-center gap-6">
      <a href="/" class="text-sm text-text-muted hover:text-text transition">Home</a>
      <a href="/eat" class="text-sm text-text-muted hover:text-text transition">Eat</a>
    </nav>
  </div>
</header>
```

#### 2.3 Create Footer Component

Create `src/components/Footer.astro`:

First, install Lucide icons:
```bash
pnpm add lucide-astro
```

Then create the component:
```astro
---
import { Twitter, Youtube, Github } from 'lucide-astro';

const currentYear = new Date().getFullYear();
const socials = [
  { name: 'X', url: 'https://x.com/craigvandotcom', Icon: Twitter },
  { name: 'YouTube', url: 'https://youtube.com/@craigvandotcom', Icon: Youtube },
  { name: 'GitHub', url: 'https://github.com/craigvandotcom', Icon: Github },
];
---

<footer class="border-t border-text/10 py-8">
  <div class="max-w-6xl mx-auto px-6 flex flex-col md:flex-row items-center justify-between gap-4">
    <p class="text-sm text-text-muted">© {currentYear} Craig van Heerden</p>
    <div class="flex items-center gap-4">
      {socials.map(({ name, url, Icon }) => (
        <a
          href={url}
          target="_blank"
          rel="noopener noreferrer"
          class="text-text-muted hover:text-accent transition"
          aria-label={name}
        >
          <Icon size={24} />
        </a>
      ))}
    </div>
    <p class="text-sm text-text-muted font-heading tracking-wide">neoMeta</p>
  </div>
</footer>
```

#### 2.4 Create Badge Component

Create `src/components/Badge.astro`:
```astro
---
interface Props {
  variant: 'live' | 'coming';
  text?: string;
}

const { variant, text } = Astro.props;
const isLive = variant === 'live';
---

<span
  class:list={[
    "inline-block px-3 py-1 text-xs font-semibold uppercase tracking-wider rounded-sm",
    isLive ? "bg-accent text-bg" : "border border-text text-text"
  ]}
>
  {text || (isLive ? 'Live' : 'Coming Soon')}
</span>
```

#### 2.5 Create PillarCard Component

Create `src/components/PillarCard.astro`:
```astro
---
import Badge from './Badge.astro';

interface Props {
  title: string;
  subtitle: string;
  icon: string;
  href?: string;
  status: 'live' | 'coming';
}

const { title, subtitle, icon, href, status } = Astro.props;
const isClickable = status === 'live' && href;
---

<div
  class:list={[
    "relative bg-bg-elevated border border-text/20 p-8 transition-all duration-200",
    isClickable && "cursor-pointer hover:border-accent hover:shadow-[0_0_30px_rgba(1,168,120,0.15)]"
  ]}
>
  <!-- Corner markers -->
  <div class="absolute top-2 left-2 w-3 h-3 border-l border-t border-text/30"></div>
  <div class="absolute bottom-2 right-2 w-3 h-3 border-r border-b border-text/30"></div>

  <!-- Badge -->
  <div class="absolute top-4 right-4">
    <Badge variant={status} />
  </div>

  <!-- Content -->
  <div class="flex flex-col items-center text-center pt-4">
    <img src={icon} alt={title} class="w-16 h-16 mb-4" />
    <h3 class="font-heading text-2xl font-semibold mb-1">{title}</h3>
    <p class="text-sm text-text-muted">{subtitle}</p>
  </div>

  {isClickable && (
    <a href={href} class="absolute inset-0" aria-label={`Go to ${title}`}></a>
  )}
</div>
```

**Note:** Badge component is imported in the frontmatter (line 1-11), not in a script tag. Astro components cannot be imported in client-side scripts.

#### 2.6 Create Button Component

Create `src/components/Button.astro`:
```astro
---
interface Props {
  variant?: 'primary' | 'secondary';
  href?: string;
  type?: 'button' | 'submit';
  class?: string;
}

const { variant = 'primary', href, type = 'button', class: className } = Astro.props;

const baseClasses = "inline-flex items-center justify-center px-6 py-3 text-sm font-medium tracking-wide transition-all duration-200";
const variantClasses = {
  primary: "border border-accent text-text hover:bg-accent hover:text-bg hover:shadow-[0_0_20px_rgba(1,168,120,0.4)]",
  secondary: "border border-text text-text hover:bg-text/10"
};
---

{href ? (
  <a href={href} class:list={[baseClasses, variantClasses[variant], className]}>
    <slot />
  </a>
) : (
  <button type={type} class:list={[baseClasses, variantClasses[variant], className]}>
    <slot />
  </button>
)}
```

#### 2.7 Create 404 Page

Create `src/pages/404.astro`:
```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import Button from '../components/Button.astro';
---

<BaseLayout title="Page Not Found - Craig van Heerden">
  <Header />

  <main id="main-content" class="pt-16 min-h-[60vh] flex items-center justify-center">
    <div class="text-center px-6">
      <h1 class="font-heading text-6xl font-bold mb-4 accent-glow">404</h1>
      <p class="text-xl text-text-muted mb-8">Page not found</p>
      <Button href="/" variant="primary">
        Return Home
      </Button>
    </div>
  </main>

  <Footer />
</BaseLayout>
```

### Deliverables
- [ ] BaseLayout.astro (with OG meta, skip link)
- [ ] Header.astro
- [ ] Footer.astro (with Lucide icons)
- [ ] Badge.astro
- [ ] PillarCard.astro
- [ ] Button.astro
- [ ] 404.astro

### Acceptance Criteria
- Components render correctly
- Styling matches design system
- Hover states work
- Responsive on mobile
- Social icons display correctly
- Skip link works for accessibility

---

## Phase 3: Home Page

### Objective
Build the complete home page with hero, pillars, and footer.

### Tasks

#### 3.1 Create Home Page

Create `src/pages/index.astro`:
```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import PillarCard from '../components/PillarCard.astro';
import Badge from '../components/Badge.astro';

const pillars = [
  {
    title: 'EAT',
    subtitle: 'Body Compass',
    icon: '/icons/pillar-eat-styled.png',
    href: '/eat',
    status: 'live' as const,
  },
  {
    title: 'MOVE',
    subtitle: 'Freedom of Movement',
    icon: '/icons/pillar-move-styled.png',
    status: 'coming' as const,
  },
  {
    title: 'MEDITATE',
    subtitle: 'Art of Stillness',
    icon: '/icons/pillar-meditate-styled.png',
    status: 'coming' as const,
  },
];
---

<BaseLayout title="Craig van Heerden - eat. move. meditate.">
  <Header />

  <main id="main-content" class="pt-16">
    <!-- Hero Section -->
    <section class="min-h-[80vh] flex flex-col items-center justify-center px-6 text-center">
      <h1 class="font-heading text-5xl md:text-7xl font-bold mb-4">
        Craig van Heerden
      </h1>
      <p class="text-2xl md:text-3xl mb-8">
        <span class="accent-glow">eat</span>.
        <span class="accent-glow">move</span>.
        <span class="accent-glow">meditate</span>.
      </p>
      <p class="text-text-muted mb-8 max-w-md">
        Building Life Operating Systems. Start with health sovereignty.
      </p>

      <!-- Newsletter Form -->
      <form class="flex flex-col sm:flex-row gap-3 w-full max-w-md">
        <input
          type="email"
          placeholder="Enter your email"
          class="flex-1 bg-transparent border border-text/30 px-4 py-3 text-text placeholder:text-text-muted focus:border-accent focus:outline-none focus:ring-2 focus:ring-accent/20"
          required
        />
        <button
          type="submit"
          class="px-6 py-3 border border-accent text-text font-medium hover:bg-accent hover:text-bg transition-all hover:shadow-[0_0_20px_rgba(1,168,120,0.4)]"
        >
          Subscribe
        </button>
      </form>
    </section>

    <!-- Pillars Section -->
    <section class="py-24 px-6">
      <div class="max-w-5xl mx-auto">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
          {pillars.map((pillar) => (
            <PillarCard {...pillar} />
          ))}
        </div>
      </div>
    </section>
  </main>

  <Footer />
</BaseLayout>
```

#### 3.2 Integrate SubstackAPI for Newsletter

Replace the basic form with SubstackAPI.com integration:
1. Go to substackapi.com
2. Enter publication name: craigvandotcom
3. Customize colors to match design
4. Copy generated code
5. Replace form in index.astro

### Deliverables
- [ ] Home page complete
- [ ] Hero section with headline + newsletter
- [ ] Three pillar cards
- [ ] EAT card links to /eat
- [ ] MOVE and MEDITATE show "Coming Soon"
- [ ] Footer renders

### Acceptance Criteria
- Page loads correctly
- Newsletter form works (or shows success message)
- EAT card is clickable
- MOVE/MEDITATE cards show Coming Soon
- Responsive on mobile

---

## Phase 4: EAT Journey Page

### Objective
Build the EAT journey page with poster download, app section, and course teaser.

### Tasks

#### 4.1 Create Poster Form Component

Create `src/components/PosterForm.astro`:
```astro
---
// Email capture form for poster download
---

<div class="bg-bg-elevated border border-text/20 p-8 max-w-lg mx-auto relative">
  <!-- Corner markers -->
  <div class="absolute top-2 left-2 w-3 h-3 border-l border-t border-text/30"></div>
  <div class="absolute bottom-2 right-2 w-3 h-3 border-r border-b border-text/30"></div>

  <h3 class="font-heading text-xl font-semibold mb-2 text-center">
    Get the Body Compass Poster
  </h3>
  <p class="text-text-muted text-center mb-6">
    Your visual guide to the red, yellow, and green food zones.
  </p>

  <form class="flex flex-col sm:flex-row gap-3" id="poster-form">
    <input
      type="email"
      name="email"
      placeholder="Enter your email"
      class="flex-1 bg-transparent border border-text/30 px-4 py-3 text-text placeholder:text-text-muted focus:border-accent focus:outline-none focus:ring-2 focus:ring-accent/20"
      required
    />
    <button
      type="submit"
      class="px-6 py-3 border border-accent text-text font-medium hover:bg-accent hover:text-bg transition-all hover:shadow-[0_0_20px_rgba(1,168,120,0.4)]"
    >
      Send Me the Poster
    </button>
  </form>

  <p class="text-xs text-text-muted text-center mt-4">
    We'll send the poster PDF to your inbox. No spam, unsubscribe anytime.
  </p>
</div>

<script>
  // Handle form submission
  // TODO: Integrate with Substack or email service
</script>
```

#### 4.2 Create EAT Journey Page

Create `src/pages/eat.astro`:
```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Header from '../components/Header.astro';
import Footer from '../components/Footer.astro';
import PosterForm from '../components/PosterForm.astro';
import Badge from '../components/Badge.astro';
import Button from '../components/Button.astro';
---

<BaseLayout
  title="Body Compass - Craig van Heerden"
  description="Nutrition sovereignty. Reclaim your intuitive relationship with food."
  ogImage="/og/eat.png"
>
  <Header />

  <main id="main-content" class="pt-16">
    <!-- Hero Section -->
    <section class="py-24 px-6 text-center">
      <div class="max-w-3xl mx-auto">
        <img
          src="/icons/pillar-eat-styled.png"
          alt="EAT"
          class="w-24 h-24 mx-auto mb-6"
        />
        <h1 class="font-heading text-4xl md:text-6xl font-bold mb-4">
          Body Compass
        </h1>
        <p class="text-xl text-text-muted">
          Nutrition sovereignty. Reclaim your intuitive relationship with food.
        </p>
      </div>
    </section>

    <!-- Poster Section -->
    <section class="py-16 px-6 bg-bg-elevated/50">
      <div class="max-w-4xl mx-auto">
        <div class="grid md:grid-cols-2 gap-8 items-center">
          <div>
            <!-- Poster preview image placeholder -->
            <div class="aspect-[3/4] bg-bg border border-text/20 flex items-center justify-center">
              <span class="text-text-muted">Poster Preview</span>
            </div>
          </div>
          <PosterForm />
        </div>
      </div>
    </section>

    <!-- App Section -->
    <section class="py-24 px-6">
      <div class="max-w-4xl mx-auto">
        <div class="flex items-center justify-center gap-3 mb-6">
          <h2 class="font-heading text-3xl font-semibold">Track with the App</h2>
          <Badge variant="live" />
        </div>
        <p class="text-text-muted text-center max-w-2xl mx-auto mb-8">
          Body Compass helps you log meals, track reactions, and build food intuition.
          Discover your personal red, yellow, and green zones.
        </p>

        <!-- App screenshot placeholder -->
        <div class="max-w-sm mx-auto mb-8">
          <div class="aspect-[9/16] bg-bg-elevated border border-text/20 flex items-center justify-center">
            <span class="text-text-muted">App Screenshot</span>
          </div>
        </div>

        <div class="flex flex-col sm:flex-row gap-4 justify-center">
          <Button href="https://bodycompass.app" variant="primary">
            Learn More
          </Button>
        </div>
      </div>
    </section>

    <!-- Course Section -->
    <section class="py-24 px-6 bg-bg-elevated/50">
      <div class="max-w-4xl mx-auto text-center">
        <div class="flex items-center justify-center gap-3 mb-6">
          <h2 class="font-heading text-3xl font-semibold">Body Compass Course</h2>
          <Badge variant="coming" />
        </div>
        <p class="text-text-muted max-w-2xl mx-auto">
          11 lessons to transform your relationship with food.
          Learn the complete Body Compass methodology and build lasting food freedom.
        </p>
      </div>
    </section>
  </main>

  <Footer />
</BaseLayout>
```

### Deliverables
- [ ] EAT journey page complete
- [ ] Poster download section with email form
- [ ] App section with link to bodycompass.app
- [ ] Course section with Coming Soon badge

### Acceptance Criteria
- Page loads correctly
- Poster form captures email (integration TBD)
- App button links to bodycompass.app
- Course shows Coming Soon
- Responsive on mobile

---

## Phase 5: Polish & Deploy

### Objective
Final polish, testing, and deployment to Vercel.

### Tasks

#### 5.1 Add Analytics

Add Umami tracking script to BaseLayout.astro head:
```html
<script
  defer
  src="https://analytics.craigvan.com/script.js"
  data-website-id="YOUR_WEBSITE_ID"
></script>
```

#### 5.2 Test All Pages

Run through checklist:
- [ ] Home page renders correctly
- [ ] EAT page renders correctly
- [ ] All links work
- [ ] Forms function (or show correct states)
- [ ] Mobile responsive
- [ ] No console errors

#### 5.3 Run Lighthouse Audit

```bash
pnpm build
pnpm preview
# Run Lighthouse in Chrome DevTools
```

Target scores:
- Performance: 90+
- Accessibility: 95+
- Best Practices: 90+
- SEO: 90+

#### 5.4 Deploy to Vercel

```bash
vercel
# Follow prompts to deploy
```

Or connect GitHub repo to Vercel for auto-deploy.

#### 5.5 Configure Custom Domain

1. In Vercel dashboard, go to project settings
2. Add custom domain: craigvan.com
3. Configure DNS records as instructed
4. Enable HTTPS (automatic)

### Deliverables
- [ ] Analytics tracking added
- [ ] All pages tested
- [ ] Lighthouse scores meet targets
- [ ] Deployed to Vercel
- [ ] Custom domain configured

### Acceptance Criteria
- Site live at craigvan.com
- Analytics tracking
- SSL enabled
- All pages functional

---

## Post-Launch Tasks

After MVP is live:

1. **Set up email automation** for poster delivery
2. **Add poster PDF** to automation
3. **Add app screenshots** to EAT page
4. **Monitor analytics** for user behavior
5. **Plan V2** (MOVE pillar)

---

## Quick Reference

### Commands

```bash
# Development
pnpm dev

# Build
pnpm build

# Preview build
pnpm preview

# Type check
pnpm astro check

# Deploy
vercel
```

### File Locations

| What | Where |
|------|-------|
| Pages | `src/pages/` |
| Components | `src/components/` |
| Layouts | `src/layouts/` |
| Styles | `src/styles/` |
| Assets | `public/` |

---

## Support

- **PRD:** See `PRD.md` for requirements
- **Design:** See `SPEC.md` for specifications
- **Dev Environment:** See `.claude/README.md`

---

**Begin with Phase 1. Execute phases in sequence.**
