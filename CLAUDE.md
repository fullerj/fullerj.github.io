# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal research/brand site for EmpiricalDefense (<https://empiricaldefense.com>), built with Jekyll 4 on the **Hydejack 9** theme (`jekyll-theme-hydejack` gem). Most work here is content (blog posts, publications, talks) plus targeted overrides of the theme's layouts/includes. There is no test suite; the build plus HTML Proofer is the validation.

## Commands

Ruby 3.2.3 (via rbenv, see `.ruby-version`), Bundler 2.2.3, gems installed locally to `vendor/bundle`.

```bash
bundle config set --local path 'vendor/bundle'
bundle _2.2.3_ install

bundle exec jekyll serve                       # dev server at http://localhost:4000, auto-rebuilds
JEKYLL_ENV=production bundle exec jekyll build --trace   # production build (what CI runs)

# Validate output the way CI does (internal links/images/scripts only)
htmlproofer ./_site --disable-external \
  --ignore-files "/CHANGELOG/,/licenses/,/LICENSE/,/NOTICE/" --checks Images,Scripts

# Free port 4000 if serve fails
pids=$(lsof -tiTCP:4000 -sTCP:LISTEN); [ -n "$pids" ] && kill -9 $pids
```

Changes to `_config.yml` are not picked up by `jekyll serve` — restart it.

## Deployment

Pushing to `main` triggers `.github/workflows/jekyll.yml`: builds on `ubuntu-24.04`, runs HTML Proofer, deploys `_site` to GitHub Pages. Things that pass locally but fail in CI:
- **Case-sensitive paths** on Linux — filenames referenced in `_data/*.yml` and templates must match exactly (e.g. `GT.png` vs `gt.png`).
- **Missing images/scripts** fail HTML Proofer and block the deploy.
- **Bundler platform errors (exit 16)** — fix with `bundle lock --add-platform x86_64-linux` (and keep `arm64-darwin-*`) and commit `Gemfile.lock`.

`_site/`, `.jekyll-cache/`, `vendor/bundle/` are generated/ignored — never edit them.

## Architecture

- **Theme overrides**: anything in `_layouts/`, `_includes/`, `_sass/` shadows the same-named file inside the Hydejack gem. To change theme behavior, copy the gem file (`bundle exec gem contents jekyll-theme-hydejack`) into the matching local path and edit it. Key local overrides: `_layouts/blog.html` and `_layouts/publication.html`, plus `_includes/components/post-blog.html`, `post-list-item.html`, `related-posts.html` (these hide dates on blog entries, restore publication metadata, and only render related posts when `related_posts:` is set explicitly in front matter).
- **Custom site-wide JS/UI** is injected via `_includes/my-body.html` / `my-head.html` (Hydejack hooks): Cloudflare email-protection re-run on Hydejack's push-state navigation, the definition modal (content from `_data/definitions.yml`), image lightbox, and inline image zoom. Because Hydejack uses push-state (SPA-style) navigation, page-level scripts must hook `hy-push-state-after` rather than relying on `DOMContentLoaded`.
- **Custom styles**: `_sass/my-style.scss`, `_sass/home.scss`.
- **Homepage**: `index.html` uses the `about` layout and renders `about.md`, so editing About changes the landing page.
- **Content sources**:
  - Blog: `_posts/` + `blog.md` (card gallery grouped by year).
  - Publications: `publications/_posts/` (template: `2025-10-13-vader.md`); PDFs in `assets/papers/`, linked with absolute paths.
  - Teaching: `teaching/_posts/`.
  - Talks, education, certifications: `_data/talks.yml`, `_data/education.yml`, `_data/certifications.yml` (rendered by `talks.md` / `about.md`).
  - Featured sections: `_featured_categories/` collection.
  - Sidebar nav: `menu:` in `_config.yml`.
- **Search**: `assets/search-data.json` is a Liquid template producing the index for the navbar search overlay. Exclude a page with `search_exclude: true`.
- **`jekyll-titles-from-headings`** is enabled but disabled for posts via `defaults`, so posts must set an explicit `title`.

## Blog post conventions

Front matter used by existing posts:

```yaml
---
layout: blog
title: "..."
description: >      # ~160 chars, used for SEO/social
  ...
abstract: >         # optional
  ...
date: 2026-05-16
categories: [posts] # required to appear on /blog/
permalink: /blog/<slug>/
tags: [...]
image:              # card image; falls back to /assets/img/logos/brand.png
  path: /assets/blogs/<slug>/<file>.png
  alt: "..."
related_posts: []   # optional; omit to hide the related-posts block
---
```

Post assets live under `assets/blogs/<slug>/`. Post bodies frequently mix raw HTML (e.g. `<h2>`) with Markdown.
