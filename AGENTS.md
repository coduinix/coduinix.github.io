# Copilot Instructions for coduinix.github.io

This is a personal blog and portfolio website built with Jekyll using the Minimal Mistakes theme, deployed to GitHub Pages.

## Quick Start

### Local Development
Run the Jekyll site locally with drafts and live reload in Docker:
```bash
docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  --publish 4000:4000 --publish 35729:35729 \
  jekyll/jekyll \
  jekyll serve --draft --livereload
```

The site will be available at `http://localhost:4000`.

### Install Dependencies
```bash
bundle install
```

## Architecture

### Core Structure
- **`_posts/`** — Published blog articles with front matter (YAML metadata)
- **`_drafts/`** — Unpublished work-in-progress content (not rendered in production)
- **`_pages/`** — Static pages like About, Blog, Articles, Talks (included in site build via `include` in `_config.yml`)
- **`_includes/`** — Reusable HTML snippets (currently overrides theme components like author profile)
- **`_layouts/`** — Custom layout templates (e.g., `talk.html` for talk pages)
- **`_talks/`** — Talk collection with custom rendering
- **`assets/`** — Images, CSS, and other static assets
- **`_config.yml`** — Jekyll configuration including theme, plugins, and defaults

### Theme
Uses **Minimal Mistakes v4.24.0** (remote theme) with the "contrast" skin. Customizations are minimal and loaded via `jekyll-remote-theme`.

### Plugins
- `jekyll-remote-theme` — Load theme from GitHub
- `jekyll-sitemap` — Auto-generate XML sitemap
- `jekyll-paginate` — Pagination for blog archives
- `jekyll-seo-tag` — SEO meta tags
- `jekyll-feed` — RSS feed generation
- `jekyll-include-cache` — Performance optimization for includes

## Content Conventions

### Blog Posts
**File format:** `_posts/YYYY-MM-DD-slug.md`

**Front matter example:**
```yaml
---
title: Article Title
excerpt: Brief summary shown in blog archives
tags:
  - tag1
  - tag2
toc: true
---
```

- Posts are automatically sorted by date (newest first)
- Excerpt separator is `<!-- more -->`
- Layout defaults to `single` with read time and sharing enabled
- Author profile shown by default on posts

### Pages
Static pages (About, Blog, Articles, etc.) use `layout: splash` or `single`. Include `permalink:` to control URL structure. They must be listed in `_config.yml` under `include: - "_pages"` to be rendered.

### Talks
Create markdown files in `_talks/` directory. The custom layout `talk.html` is automatically applied (defined in `_config.yml` defaults).

## Build & Deployment

### GitHub Pages Workflow
Automatically deploys to GitHub Pages on every push to `main` branch. The workflow:
1. Checks out code
2. Builds Jekyll site to `_site/` directory
3. Uploads artifact to GitHub Pages

View workflow: `.github/workflows/jekyll.yml`

### Local Testing Before Commit
Always test locally with the Docker command above. Verify:
- Content renders without errors
- Links work correctly
- Front matter is valid YAML

## Key Configuration

**`_config.yml` highlights:**
- `timezone: Europe/Amsterdam` — Blog post timezone
- `permalink: /:categories/:year/:month/:day/:title` — URL slug format
- `sass.style: compressed` — CSS minification
- `tag_archive.path: /tags/` — Tag page location
- Google Analytics ID: `G-19Y7T9TVQW`

## Common Tasks

### Add a Blog Post
1. Create `_posts/YYYY-MM-DD-slug.md`
2. Add required front matter (title, excerpt, tags)
3. Write content
4. Test locally with Docker
5. Commit and push to `main`

### Add a Draft
Same as above but use `_drafts/` instead of `_posts/`. It will render locally with `--draft` flag but not in production.

### Customize Content
- Override theme includes by creating files in `_includes/` (currently has `author-profile-custom-links.html`)
- Modify site-wide defaults in `_config.yml` (collections, front matter defaults, etc.)

### Update Theme or Plugins
Edit `Gemfile` or `remote_theme` in `_config.yml`. Run `bundle install` locally to test.

## Dependencies

- **Ruby** with Bundler (managed by Docker image `jekyll/jekyll`)
- **Node.js 25.9.0+** (declared in `package.json`, used if needed for asset pipeline)
- **jekyll/jekyll Docker image** (recommended for local testing)
