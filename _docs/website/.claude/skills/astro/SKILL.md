---
name: astro
description: Astro 5 framework patterns and best practices for neoMeta website. Component structure, routing, image optimization, View Transitions. Use when working with .astro files or Astro-specific features.
---

# Astro Framework Skill

**Purpose:** Guide Astro 5 framework usage and patterns
**Domain:** .astro components, routing, static generation, optimization
**Status:** Complete

---

## When to Use This Skill

**Intent Triggers:**

- Working with `.astro` files
- Creating pages or routes
- Using Astro components
- Image optimization questions
- View Transitions implementation
- Build/SSG configuration

**When NOT to Use:**

- Styling/design questions (use `design-system` skill)
- Content writing (use `writing-guidelines` skill)
- General TypeScript (covered in `CORE`)

---

## Core Astro Concepts

### 1. Component Anatomy

```astro
---
// Component Frontmatter (JavaScript/TypeScript)
// This runs at build time, never in the browser

// 1. Imports
import BaseLayout from '@/layouts/BaseLayout.astro';
import Button from '@/components/Button.astro';

// 2. Props interface
interface Props {
  title: string;
  description?: string;
  publishDate?: Date;
}

// 3. Destructure props with defaults
const {
  title,
  description = 'Default description',
  publishDate
} = Astro.props;

// 4. Build-time logic/data fetching
const currentYear = new Date().getFullYear();

// 5. Functions (build-time only)
function formatDate(date: Date) {
  return date.toLocaleDateString('en-US');
}
---

<!-- Template (HTML-like, rendered to static HTML) -->
<BaseLayout {title} {description}>
  <main class="container mx-auto px-6 py-24">
    <h1 class="text-h1 mb-4">{title}</h1>
    {description && <p class="text-body text-text-muted">{description}</p>}
    {publishDate && <time>{formatDate(publishDate)}</time>}

    <Button>Click me</Button>
  </main>
</BaseLayout>

<!-- Optional: Scoped styles -->
<style>
  /* Scoped to this component only */
  main {
    /* Custom styles if Tailwind isn't enough */
  }
</style>
```

### 2. Zero JavaScript by Default

**Key principle:** Astro ships zero JavaScript by default. All components render to static HTML.

```astro
---
// ✅ CORRECT: Static component (no JS sent to browser)
interface Props {
  text: string;
}

const { text } = Astro.props;
---

<button class="border border-accent px-6 py-3">
  {text}
</button>

<!-- Static HTML, no interactivity needed -->
```

**Only add client-side JS when truly necessary (rare for this site):**

```astro
---
// ❌ ONLY USE IF ABSOLUTELY NECESSARY
import InteractiveComponent from '@/components/InteractiveComponent.tsx';
---

<!-- This sends JS to the browser -->
<InteractiveComponent client:load />
```

**Client directives (use sparingly):**

- `client:load` - Load immediately
- `client:idle` - Load when browser idle
- `client:visible` - Load when visible in viewport
- `client:only` - Only run on client (no SSR)

**For this project:** We should NOT need client directives. If you think you do, ask Craig first.

---

## Routing

### File-Based Routing

```
src/pages/
├── index.astro          → /
├── philosophy.astro     → /philosophy
├── systems.astro        → /systems
├── poster.astro         → /poster
└── blog/
    ├── index.astro      → /blog
    └── [slug].astro     → /blog/[slug] (dynamic)
```

### Static Routes (Current Website)

```astro
---
// src/pages/philosophy.astro
import BaseLayout from '@/layouts/BaseLayout.astro';

const pageTitle = 'Philosophy';
const pageDescription = 'The WHY behind neoMeta: time preference, AI alignment, integrity.';
---

<BaseLayout title={pageTitle} description={pageDescription}>
  <main>
    <!-- Page content -->
  </main>
</BaseLayout>
```

### Dynamic Routes (Future, if needed)

```astro
---
// src/pages/blog/[slug].astro

// getStaticPaths generates all routes at build time
export async function getStaticPaths() {
  const posts = await fetchPosts(); // Your data source

  return posts.map(post => ({
    params: { slug: post.slug },
    props: { post }
  }));
}

// Access the post data
const { post } = Astro.props;
---

<BaseLayout title={post.title}>
  <article>
    <h1>{post.title}</h1>
    <div set:html={post.content} />
  </article>
</BaseLayout>
```

---

## Layouts

### BaseLayout Pattern

```astro
---
// src/layouts/BaseLayout.astro
import Header from '@/components/Header.astro';
import Footer from '@/components/Footer.astro';

interface Props {
  title: string;
  description?: string;
  ogImage?: string;
}

const {
  title,
  description = 'eat. move. meditate. A framework for human sovereignty.',
  ogImage = '/og-image.png'
} = Astro.props;

const canonicalURL = new URL(Astro.url.pathname, Astro.site);
---

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />

    <!-- Primary Meta Tags -->
    <title>{title} | Craig van Heerden</title>
    <meta name="title" content={title} />
    <meta name="description" content={description} />

    <!-- Open Graph / Facebook -->
    <meta property="og:type" content="website" />
    <meta property="og:url" content={canonicalURL} />
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:image" content={ogImage} />

    <!-- Twitter -->
    <meta property="twitter:card" content="summary_large_image" />
    <meta property="twitter:url" content={canonicalURL} />
    <meta property="twitter:title" content={title} />
    <meta property="twitter:description" content={description} />
    <meta property="twitter:image" content={ogImage} />

    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet" />

    <!-- Analytics (Umami) -->
    <script
      defer
      src="https://analytics.craigvan.com/script.js"
      data-website-id={import.meta.env.PUBLIC_UMAMI_WEBSITE_ID}
    ></script>
  </head>
  <body class="bg-bg text-text antialiased">
    <Header />

    <slot />

    <Footer />
  </body>
</html>
```

### Usage

```astro
---
// src/pages/index.astro
import BaseLayout from '@/layouts/BaseLayout.astro';
---

<BaseLayout title="Home">
  <main>
    <!-- Page content goes in the <slot /> -->
  </main>
</BaseLayout>
```

---

## Image Optimization

### Astro Image Component

```astro
---
import { Image } from 'astro:assets';
import heroImage from '@/assets/hero.jpg'; // Import from src/assets/
---

<!-- ✅ CORRECT: Optimized with Astro Image -->
<Image
  src={heroImage}
  alt="Hero image description"
  width={1920}
  height={1080}
  format="webp"
  quality={80}
  loading="eager"
  class="w-full h-auto"
/>

<!-- For multiple formats/sizes (responsive) -->
<Image
  src={heroImage}
  alt="Hero image"
  widths={[320, 640, 960, 1280, 1920]}
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 80vw, 1200px"
  format="webp"
  quality={80}
  class="w-full h-auto"
/>

<!-- ❌ WRONG: Standard img tag (no optimization) -->
<img src="/hero.jpg" alt="Hero" />
```

### Public vs Assets

**Use `src/assets/` for images you want optimized:**

```
src/assets/
├── hero.jpg      → import heroImage from '@/assets/hero.jpg'
├── about.png     → import aboutImage from '@/assets/about.png'
```

**Use `public/` for:**

- Favicon
- Icons (SVG)
- Logo (SVG)
- Assets that don't need optimization

```
public/
├── favicon.svg
├── icons/
│   ├── eat.svg
│   ├── move.svg
│   └── meditate.svg
└── logo.svg
```

---

## View Transitions (Page Animations)

**Enable smooth page transitions:**

```astro
---
// src/layouts/BaseLayout.astro
import { ViewTransitions } from 'astro:transitions';
---

<html>
  <head>
    <ViewTransitions />
  </head>
  <body>
    <!-- ... -->
  </body>
</html>
```

**Persist elements across navigation:**

```astro
<!-- Keep header in place during transitions -->
<Header transition:persist />

<!-- Animate specific elements -->
<div transition:animate="slide">
  <!-- Content -->
</div>
```

**Built-in animations:**

- `fade` - Fade in/out
- `slide` - Slide from bottom
- `none` - No animation

---

## Environment Variables

### Access in Frontmatter

```astro
---
// Build-time access (private, not exposed to browser)
const apiKey = import.meta.env.SECRET_API_KEY;

// Runtime access (public, exposed to browser)
const publicId = import.meta.env.PUBLIC_UMAMI_WEBSITE_ID;
---

<!-- ✅ PUBLIC_ vars can be used in template -->
<script data-website-id={import.meta.env.PUBLIC_UMAMI_WEBSITE_ID}></script>

<!-- ❌ NEVER expose non-PUBLIC vars to browser -->
<script data-api-key={import.meta.env.SECRET_API_KEY}></script>
```

### .env File

```bash
# .env (gitignored)
# Private (only available in .astro frontmatter)
SECRET_API_KEY=secret123

# Public (available in browser)
PUBLIC_UMAMI_WEBSITE_ID=abc123
PUBLIC_SUBSTACK_PUBLICATION=craigvan
```

---

## Data Fetching (Build Time)

```astro
---
// All data fetching happens at build time, never in browser

// ✅ CORRECT: Fetch at build time
const response = await fetch('https://api.example.com/posts');
const posts = await response.json();

// Import from local files
import { posts } from '@/data/posts.json';

// Read from filesystem (Node.js APIs available)
import fs from 'node:fs/promises';
const content = await fs.readFile('./content.md', 'utf-8');
---

<ul>
  {posts.map(post => (
    <li>{post.title}</li>
  ))}
</ul>
```

---

## Slot Pattern

**Pass content to components:**

```astro
---
// src/components/Card.astro
interface Props {
  title: string;
}

const { title } = Astro.props;
---

<div class="card">
  <h3>{title}</h3>
  <slot /> <!-- Content goes here -->
</div>
```

**Usage:**

```astro
<Card title="Card Title">
  <p>This content goes in the slot</p>
  <button>Click me</button>
</Card>
```

**Named slots:**

```astro
---
// Component with multiple slots
---

<div>
  <header>
    <slot name="header" />
  </header>
  <main>
    <slot /> <!-- Default slot -->
  </main>
  <footer>
    <slot name="footer" />
  </footer>
</div>
```

**Usage:**

```astro
<MyComponent>
  <h1 slot="header">Header</h1>
  <p>Main content (default slot)</p>
  <p slot="footer">Footer</p>
</MyComponent>
```

---

## TypeScript

### Props Interface (Always Define)

```astro
---
// ✅ CORRECT: Typed props
interface Props {
  title: string;
  description?: string; // Optional
  tags?: string[];
}

const { title, description, tags = [] } = Astro.props;
---

---
// ❌ WRONG: Untyped props
const { title, description } = Astro.props;
---
```

### Type Checking

```bash
# Type check all .astro files
pnpm astro check

# Watch mode
pnpm astro check --watch
```

---

## Common Patterns

### Newsletter Form (SubstackAPI)

```astro
---
// src/components/NewsletterForm.astro
const publicationName = import.meta.env.PUBLIC_SUBSTACK_PUBLICATION;
---

<form
  action={`https://substackapi.com/widget.js`}
  method="POST"
  class="flex gap-3 max-w-lg mx-auto"
>
  <input
    type="email"
    name="email"
    placeholder="Enter your email"
    required
    class="flex-1 bg-transparent border border-text/30 text-text px-4 py-3"
  />
  <button
    type="submit"
    class="border border-accent text-text px-6 py-3 hover:bg-accent hover:text-bg transition-all"
  >
    Join
  </button>
</form>

<script
  src="https://substackapi.com/widget.js"
  data-publication={publicationName}
  async
></script>
```

### Conditional Rendering

```astro
---
const isLive = true;
const badge = 'LIVE';
---

<!-- If statement -->
{isLive && (
  <span class="badge">LIVE</span>
)}

<!-- If-else -->
{badge === 'LIVE' ? (
  <span class="bg-accent text-bg px-3 py-1">LIVE</span>
) : (
  <span class="border border-text text-text px-3 py-1">COMING</span>
)}
```

### Lists/Mapping

```astro
---
const items = ['EAT', 'MOVE', 'MEDITATE'];
---

<ul>
  {items.map(item => (
    <li>{item}</li>
  ))}
</ul>
```

---

## Build & Deploy

### Build Command

```bash
# Build static site to dist/
pnpm build

# Preview built site
pnpm preview
```

### Vercel Configuration

```json
// vercel.json (optional, Vercel auto-detects Astro)
{
  "buildCommand": "pnpm build",
  "outputDirectory": "dist",
  "installCommand": "pnpm install"
}
```

---

## Debugging

### Dev Server

```bash
# Start dev server (port 4321)
pnpm dev

# Open in browser
open http://localhost:4321
```

### Check Console

Open browser DevTools → Console tab. Look for:

- Build warnings
- JavaScript errors
- Asset loading issues

### Build Errors

```bash
# See detailed error output
pnpm build

# Common issues:
# - TypeScript errors → run pnpm astro check
# - Missing images → check imports
# - Syntax errors → check .astro file structure
```

---

## Performance Best Practices

### 1. Keep JavaScript Minimal

```astro
<!-- ✅ CORRECT: Static HTML -->
<button class="border border-accent px-6 py-3">
  Click me
</button>

<!-- ❌ WRONG: Unnecessary client JS -->
<InteractiveButton client:load />
```

### 2. Optimize Images

```astro
<!-- ✅ Use Astro Image component -->
<Image src={hero} alt="Hero" format="webp" quality={80} />

<!-- ❌ Plain img tags -->
<img src="/hero.jpg" alt="Hero" />
```

### 3. Lazy Load Below Fold

```astro
<Image
  src={belowFold}
  alt="Below fold image"
  loading="lazy"
  format="webp"
/>
```

### 4. Preload Critical Assets

```astro
<head>
  <link rel="preload" href="/fonts/space-grotesk.woff2" as="font" type="font/woff2" crossorigin />
</head>
```

---

## Common Mistakes

| Mistake                          | Fix                                      |
| -------------------------------- | ---------------------------------------- |
| Using `client:load` unnecessarily| Remove - use static HTML                 |
| Forgetting Props interface       | Always define `interface Props`          |
| Mixing frontmatter and template  | Keep JS in frontmatter, HTML in template |
| Using browser APIs in frontmatter| Frontmatter runs at build time (Node.js) |
| Not optimizing images            | Use `<Image>` component from astro:assets|

---

## Reference

**Official Docs:** https://docs.astro.build/
**Astro Syntax:** https://docs.astro.build/en/core-concepts/astro-syntax/
**Image Optimization:** https://docs.astro.build/en/guides/images/

---

**Remember:** Astro's strength is shipping zero JavaScript by default. For this project, we should maintain that. Every component should be static HTML unless there's a compelling reason for interactivity.
