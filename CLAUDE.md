# Project Context for Claude Code

## Project Overview

This is a personal blog built with Astro Cactus, a modern blog theme. The site features blog posts, notes, tags, and search functionality.

## Tech Stack

- **Framework**: Astro v5
- **Styling**: Tailwind CSS v4
- **Content**: Markdown/MDX with Content Collections
- **Type Safety**: TypeScript
- **Code Quality**: Biome (linting), Prettier (formatting)
- **Search**: Pagefind (static search)
- **Package Manager**: pnpm
- **Features**:
  - Auto-generated OG images (Satori)
  - RSS feeds (posts and notes)
  - Sitemap & robots.txt
  - Webmentions
  - Dark/light theme
  - Syntax highlighting (Expressive Code)

## Directory Structure

```
src/
├── components/        # Astro components
│   ├── blog/         # Blog-specific components (PostPreview, TOC, etc.)
│   ├── layout/       # Layout components (Header, Footer)
│   └── note/         # Note-specific components
├── content/          # Content Collections (managed by Astro)
│   ├── post/         # Blog posts (markdown/mdx)
│   ├── note/         # Quick notes (markdown/mdx)
│   └── tag/          # Tag pages (optional overrides)
├── layouts/          # Page layouts (Base, BlogPost)
├── pages/            # Astro pages & routes
│   ├── posts/        # Dynamic post routes
│   ├── notes/        # Dynamic note routes
│   ├── tags/         # Dynamic tag routes
│   └── og-image/     # OG image generation
├── plugins/          # Remark plugins (reading-time, admonitions, github-card)
├── styles/           # Global CSS & component styles
├── utils/            # Utility functions (date, TOC, webmentions, etc.)
└── site.config.ts    # Main site configuration
```

## Key Patterns & Conventions

### Content Collections

Content is managed through Astro Content Collections, configured in `src/content.config.ts`:

- **Posts**: Feature-length blog articles with frontmatter validation
- **Notes**: Shorter, quick thoughts or updates
- **Tags**: Optional tag page overrides

### Frontmatter Requirements

**Post Frontmatter** (required fields):
```yaml
---
title: "Max 60 chars"
description: "Min 50, max 160 chars"
publishDate: "YYYY-MM-DD"
tags: ["optional", "tags"]
draft: false # Set true to exclude from production
---
```

**Note Frontmatter**:
```yaml
---
title: "Max 60 chars"
publishDate: "ISO 8601 format"
description: "Optional"
---
```

### File Naming

- Content files use the filename as the URL slug
- Component files use PascalCase (e.g., `PostPreview.astro`)
- Utility files use camelCase (e.g., `generateToc.ts`)

### Styling

- Tailwind v4 with custom configuration in `tailwind.config.ts`
- Global styles in `src/styles/global.css`
- Component-specific styles in `src/styles/components/`
- Dark/light theme managed via CSS variables and Tailwind classes

### Theme System (Time-Based)

The theme automatically switches based on the viewer's local time:

**Default Behavior:**
- **Light theme**: 7 AM - 7 PM (07:00 - 19:00)
- **Dark theme**: 7 PM - 7 AM (19:00 - 07:00)
- Theme is checked every minute and updates automatically

**Manual Override:**
- Single-click the theme toggle button to manually switch themes
- Manual selection is saved to localStorage and persists across sessions
- Double-click the theme toggle button to reset to automatic time-based behavior

**Implementation:**
- `src/components/ThemeProvider.astro` - Main theme logic with time-based detection
- `src/components/ThemeToggle.astro` - Toggle button with double-click reset feature

**Configuration:**
To change the daytime hours, edit `src/components/ThemeProvider.astro`:
```javascript
const LIGHT_THEME_START_HOUR = 7; // 7 AM
const LIGHT_THEME_END_HOUR = 19;   // 7 PM
```

### Custom Remark Plugins

Located in `src/plugins/`:
- **remark-reading-time**: Calculates reading time for posts
- **remark-admonitions**: Adds note/warning/tip callout blocks
- **remark-github-card**: Embeds GitHub repository cards

## Configuration Files

- **src/site.config.ts**: Main site settings (title, description, author, social links)
- **astro.config.ts**: Astro configuration & integrations
- **biome.json**: Linting rules
- **tailwind.config.ts**: Tailwind customization

## Development Workflow

### Commands
```bash
pnpm dev        # Start dev server (localhost:3000)
pnpm build      # Build production site
pnpm postbuild  # Generate Pagefind search index (run after build)
pnpm preview    # Preview production build
pnpm format     # Format code with Biome & Prettier
pnpm lint       # Lint with Biome
pnpm check      # Type-check with Astro
```

### Build Process

1. `pnpm build` compiles site to `dist/`
2. `pnpm postbuild` runs Pagefind to generate search index
3. Both steps are required for full production deployment

## Important Notes

### Content Management

- New posts go in `src/content/post/` as `.md` or `.mdx` files
- New notes go in `src/content/note/`
- Tags are auto-generated from post frontmatter
- Tag overrides can be added to `src/content/tag/` (filename must match tag name)

### OG Images

- Automatically generated for all posts via Satori
- Generation logic in `src/pages/og-image/[slug].png.ts`
- Can override per-post with `ogImage` frontmatter property

### Search

- Pagefind only works after build (`pnpm build && pnpm postbuild`)
- Indexes content marked with `data-pagefind-body` attribute
- Currently indexes posts and notes only
- Supports filtering by tags

### Theme Configuration

- Edit `src/site.config.ts` to change site metadata
- Update `src/components/SocialList.astro` for social links
- Replace files in `/public` for branding (icon.svg, social-card.png)

## Common Tasks

### Adding a New Post
1. Create `src/content/post/my-post.md`
2. Add required frontmatter (title, description, publishDate)
3. Write content in markdown/mdx
4. Post will be available at `/posts/my-post`

### Adding a New Page
1. Create file in `src/pages/` (e.g., `contact.astro`)
2. Use `Base` layout for consistency
3. Page will be available at `/contact`

### Modifying Styles
1. Global styles: Edit `src/styles/global.css`
2. Tailwind config: Edit `tailwind.config.ts`
3. Component styles: Use Tailwind classes or add to `src/styles/components/`

## Version Information

This project is based on Astro Theme Cactus v6.10.0.
Original theme: https://github.com/chrismwilliams/astro-theme-cactus
