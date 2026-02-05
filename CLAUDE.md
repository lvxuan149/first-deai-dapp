# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Next.js documentation site built with [Fumadocs](https://fumadocs.vercel.app). The main application code resides in the `my-app/` directory.

## Development Commands

```bash
cd my-app
npm run dev      # Start development server with Turbo (http://localhost:3000)
npm run build    # Build for production
npm run start    # Start production server
```

## Architecture

### Content Management (MDX)

- **Content Directory**: `content/docs/` - All documentation files are MDX format
- **MDX Configuration**: `source.config.ts` - Defines content collections via `defineDocs()`
- **Source Loader**: `lib/source.ts` - Exports `source` object with `pageTree` for navigation and `getPage()` for retrieving pages
- **Postinstall Hook**: `fumadocs-mdx` runs automatically to generate `.source/` directory with typed content metadata

### App Structure (Next.js App Router)

- **Root Layout**: `app/layout.tsx` - Global layout with `RootProvider` from Fumadocs UI
- **Home Layout**: `app/(home)/layout.tsx` - Uses `HomeLayout` for landing pages
- **Docs Layout**: `app/docs/layout.tsx` - Uses `DocsLayout` with `source.pageTree` for documentation sidebar
- **Docs Pages**: `app/docs/[[...slug]]/page.tsx` - Dynamic route that renders MDX content, handles 404s, and generates metadata

### Shared Configuration

- **Base Layout Options**: `app/layout.config.tsx` - Contains `baseOptions` shared between Home and Docs layouts (nav title, links)
- **MDX Components**: `mdx-components.tsx` - Custom MDX component mapping, extends `fumadocs-ui/mdx`

### Styling

- **Tailwind CSS v4** with PostCSS
- **Global CSS**: `app/global.css` - Imports Tailwind, Fumadocs neutral and preset styles
- **Font**: Inter (Google Fonts)

### Search API

- **Route**: `app/api/search/route.ts` - Exports `GET` handler powered by `createFromSource(source)` for full-text search

### Next.js Config

- `next.config.mjs` - Wrapped with Fumadocs MDX via `createMDX()`

### TypeScript Path Aliases

- `@/.source` → `.source/index.ts` (auto-generated from MDX content)
- `@/*` → `/*` (project root relative)

## Key Concepts

### Adding New Documentation

1. Create `.mdx` files in `content/docs/`
2. Use frontmatter for `title` and `description`
3. Run `npm run postinstall` or let dev server auto-generate types in `.source/`

### Fumadocs Components

MDX files support special components like `<Card>` and `<Cards>` (see `content/docs/index.mdx`). Refer to Fumadocs docs for available components.
