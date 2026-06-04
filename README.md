# Astro Blog

A fast, content-focused blog template built with Astro, MDX, and Tailwind CSS. Zero JavaScript shipped to the client by default. Includes RSS feed, sitemap, tag filtering, and draft support.

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | [Astro](https://astro.build) v4 |
| Content | MDX (Markdown + JSX) via `@astrojs/mdx` |
| Styling | [Tailwind CSS](https://tailwindcss.com) v3 + `@tailwindcss/typography` |
| SEO | `@astrojs/sitemap` + `@astrojs/rss` |
| Types | TypeScript (strict mode) |

## Getting Started

```bash
npm install
npm run dev        # http://localhost:4321
```

```bash
npm run build      # outputs to dist/
npm run preview    # preview the production build locally
npm run lint       # type-check without emitting files
```

## Project Structure

```
astro-blog/
├── src/
│   ├── components/
│   │   ├── Header.astro          # Site nav: home, blog, RSS link
│   │   ├── Footer.astro          # Copyright footer with dynamic year
│   │   ├── FormattedDate.astro   # <time> element with localized date
│   │   └── PostCard.astro        # Post preview: title, date, description, tags
│   ├── content/
│   │   ├── blog/                 # Blog posts (.mdx files)
│   │   │   └── hello-world.mdx
│   │   └── config.ts             # Zod schema for blog frontmatter
│   ├── layouts/
│   │   └── BaseLayout.astro      # Master HTML template (meta, OG, RSS link)
│   ├── pages/
│   │   ├── index.astro           # Homepage — 5 most recent posts
│   │   ├── blog/
│   │   │   ├── index.astro       # Full post archive
│   │   │   ├── [slug].astro      # Individual post page
│   │   │   └── tags/
│   │   │       └── [tag].astro   # Posts filtered by tag
│   │   └── rss.xml.ts            # RSS 2.0 feed endpoint
│   └── env.d.ts
├── public/
│   └── favicon.svg
├── astro.config.mjs
├── tailwind.config.mjs
└── tsconfig.json
```

## Writing Posts

Create a `.mdx` file in `src/content/blog/`. All fields except `updatedDate` are required.

```mdx
---
title: My Post Title
description: A short description shown in previews and meta tags.
pubDate: 2024-06-01
updatedDate: 2024-06-15   # optional
tags: [astro, tutorial]   # optional, defaults to []
draft: false              # set true to hide from all listings
---

Your post content here. You can use **Markdown** and <JSXComponents />.
```

Draft posts are excluded from the homepage, blog index, tag pages, and RSS feed. They are not built in production.

## Content Schema

Defined in `src/content/config.ts` using Zod:

| Field | Type | Required | Default |
|---|---|---|---|
| `title` | `string` | yes | — |
| `description` | `string` | yes | — |
| `pubDate` | `Date` | yes | — |
| `updatedDate` | `Date` | no | — |
| `tags` | `string[]` | no | `[]` |
| `draft` | `boolean` | no | `false` |

## Pages & Routes

| Route | Source | Description |
|---|---|---|
| `/` | `pages/index.astro` | Homepage, shows 5 most recent posts |
| `/blog` | `pages/blog/index.astro` | Full archive, all published posts |
| `/blog/[slug]` | `pages/blog/[slug].astro` | Individual post with full content |
| `/blog/tags/[tag]` | `pages/blog/tags/[tag].astro` | Posts filtered by a single tag |
| `/rss.xml` | `pages/rss.xml.ts` | RSS 2.0 feed of all published posts |
| `/sitemap-index.xml` | auto-generated | Sitemap (via `@astrojs/sitemap`) |

All routes are statically generated at build time.

## Configuration

### Site URL

Update `site` in `astro.config.mjs` before deploying — it's used for canonical URLs, OG tags, sitemap, and RSS links:

```js
// astro.config.mjs
export default defineConfig({
  site: 'https://yourdomain.com',
  // ...
})
```

### Tailwind Theme

Extend the theme in `tailwind.config.mjs`:

```js
theme: {
  extend: {
    colors: {
      brand: '#your-color',
    },
  },
},
```

### TypeScript Path Aliases

`@/*` maps to `src/*`. Use it for clean imports:

```ts
import PostCard from '@/components/PostCard.astro'
```

## Customization Checklist

- [ ] Set `site` URL in `astro.config.mjs`
- [ ] Update site name in `src/components/Header.astro`
- [ ] Update RSS feed title in `src/pages/rss.xml.ts`
- [ ] Replace `public/favicon.svg`
- [ ] Add your first post to `src/content/blog/`
- [ ] Adjust theme colors in `tailwind.config.mjs`

## Key Features

- **Zero client JS** — pure HTML/CSS output unless you explicitly add scripts
- **Type-safe content** — frontmatter validated at build time via Zod
- **MDX** — use Astro/React components inside Markdown
- **Draft posts** — write without publishing; excluded from all listings and feeds
- **Tag system** — posts tagged and browsable at `/blog/tags/[tag]`
- **RSS feed** — auto-generated at `/rss.xml`
- **Sitemap** — auto-generated, ready for Google Search Console
- **SEO** — canonical URLs, Open Graph meta, RSS `<link>` in `<head>`
- **Responsive** — mobile-first layout via Tailwind CSS
- **Prose styling** — `@tailwindcss/typography` for beautifully rendered post content
