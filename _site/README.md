# EmpiricalDefense Website

EmpiricalDefense is a personal brand site built with Jekyll and the Hydejack theme. The repository tracks the content, configuration, and styling used on <https://empiricaldefense.com> (local preview available at <http://localhost:4000/> when running the development server).

---

## Requirements

These steps mirror the working macOS setup. Adapt as needed for Linux or Windows:

1. Install Xcode Command Line Tools (if missing):
   ```bash
   xcode-select --install
   ```
2. Optionally install Homebrew (<https://brew.sh/>) to simplify package management.
3. Install `rbenv` and Ruby build tooling:
   ```bash
   brew install rbenv ruby-build
   ```
4. Initialise `rbenv` for zsh (run once, then restart your shell):
   ```bash
   echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
   exec zsh
   ```

---

## First-time Setup

From the project root (`fullerj.github.io`):

```bash
# Install and select the Ruby version used by the site
rbenv install 3.2.3
rbenv local 3.2.3
rbenv rehash
ruby -v

# Install the Bundler version locked in Gemfile.lock
gem install bundler:2.2.3
rbenv rehash

# Install gems locally in vendor/bundle
bundle config set --local path 'vendor/bundle'
bundle _2.2.3_ install

# Run the development server
bundle exec jekyll serve
```

Navigate to <http://localhost:4000/> to preview the site. Jekyll rebuilds automatically when files change.

---

## Project Structure

Key paths and how they are used:

| Area | Location | Notes |
| --- | --- | --- |
| Homepage | `index.html` | Uses the `about` layout and renders the full content of `about.md`. Any updates to About automatically appear on the landing page. |
| About | `about.md` | Primary biography plus education, certifications, highlights, and table of contents. |
| Service | `service.md` | Volunteer, reviewer, and outreach activities. Linked from the sidebar navigation. |
| Blog | `_posts/` (blog entries) + `blog.md` | Posts using `layout: blog` and `categories: [posts]` appear on `/blog/`. Each post should define an `image` block (`path`/`alt`) for the homepage gallery and, optionally, a custom `permalink` such as `/blog/my-title/`. |
| Publications | `publications/_posts/` | Each publication entry includes optional metadata (`conference`, `pdf_url`, etc.). Use `2025-10-13-vader.md` as a template. |
| Teaching | `teaching/_posts/` | Teaching history. Cards feed the Teaching page, and education details live on the About page. |
| Talks | `_data/talks.yml` | YAML list of talks rendered by `talks.md`. Entries support title, event, location, date, and optional description/resources. |
| Education | `_data/education.yml` | Structured list of degrees used to render the Education cards on About (logo, institution, location, advisor, thesis/dissertation). |
| Certifications | `_data/certifications.yml` | Central store for certification badges displayed on About, keeping logos/titles consistent. |
| Navigation | `_config.yml` (`menu:`) | Sidebar links (Blog, Publications, Talks, Teaching, Service, About). |
| Search data | `assets/search-data.json` | Liquid template that emits the JSON index consumed by the navbar search overlay. |
| Styling | `_sass/my-style.scss`, `_sass/home.scss` | Custom SCSS for cards, dark mode tweaks, homepage spacing, etc. |
| Branding | `assets/img/…` | Logos for institutions, certifications, and the accent image (`accent_image` in `_config.yml`). |
| Favicons | `assets/icons/` | Provide `favicon.ico` and optional PWA sizes; set `favicon:` in `_config.yml` if you change the filename. |
| Layout overrides | `_layouts/blog.html`, `_layouts/publication.html`, `_includes/components/post-blog.html`, `_includes/components/post-list-item.html`, `_includes/components/related-posts.html` | Custom templates that remove dates from blog entries, restore publication metadata, and ensure related posts render only when explicitly listed in front matter. |

---

## Content Guidelines

### Search Overlay
- Click the magnifying-glass icon in the top nav to open the inline search dialog. Results update live as you type and stay within the current page.
- The overlay uses the generated `assets/search-data.json` file. It includes posts, pages, and collection documents unless front matter sets `search_exclude: true`.
- Regenerate the site (via `bundle exec jekyll build` or `bundle exec jekyll serve`) whenever you add or update content so the search index stays current.
- To remove specific items from search results, add `search_exclude: true` to the page or post front matter.

### Favicons
- Place your custom icons in `assets/icons/`. At minimum provide a square `favicon.ico` (PNG inside the ICO container) and rebuild so `_site/assets/icons/` picks up the new file.
- Optional sizes (e.g., `icon-192x192.png`) are used for PWA/touch support. Replace the defaults with matching artwork if you want consistent branding.
- If you rename the favicon, point `_config.yml` at it:  
  ```yaml
  favicon: /assets/icons/favicon.ico
  ```
  When you update the icon, do a hard refresh (or open the site in a private window) so the browser picks up the new asset.

### Publications
- Store PDFs in `assets/papers/` and link with absolute paths (e.g., `/assets/papers/ccs25.pdf`).
- Include `venue` to add a bracketed label on the index (e.g., `[ACM CCS]`).
- Optional arrays: `media_links`, `artifact_badges`, `tags`.

### Teaching & Education
- Teaching posts (`teaching/_posts/*.md`) appear under `/teaching/` and feed the About-page highlights.
- Degrees and certifications are sourced from `_data/education.yml` and `_data/certifications.yml`. Update those data files to edit or add entries; the About page renders them automatically.

### Service
- Service content is written directly in `service.md`. Add new volunteer or committee roles by editing the appropriate sections.

### Blog
- Create new entries under `_posts/` with front matter similar to:
  ```yaml
  ---
  layout: blog
  title: "From Anomalous Traffic to Spycraft 2.0"
  date: 2025-10-18
  categories: [posts]
  permalink: /blog/spycraft-2-0/
  image:
    path: /assets/blogs/spycraft/ddr.jpg
    alt: Spycraft dead drop resolver workflow
  ---
  ```
- The `/blog/` page (see `blog.md`) renders a card gallery grouped by year. Cards fall back to `/assets/img/logos/brand.png` if no `image` is provided.
- Related posts appear only when a page sets `related_posts:` to an array of post paths or URLs. Leave the field empty (or omit it) to hide the related-posts block completely.

---

## Deployment Notes

- The `_site/` folder is generated by `bundle exec jekyll build` or `jekyll serve`. Deploy that directory (or activate GitHub Pages / Netlify) to publish updates.
- Update DNS records (CNAME or A records) once hosting is configured for your custom domain.
- Remove or adjust pagination settings in `_config.yml` if you are not using Hydejack’s blog pagination (currently disabled).
- If you remove `vendor/bundle/` (for example via `git clean -fd`), reinstall dependencies with `bundle install` so required gems (e.g., `public_suffix`) are restored before serving the site.
- After cleanups, run:
  ```bash
  bundle install
  bundle exec jekyll build
  ```
- For GitHub Pages hosting, set **Settings → Pages → Build and deployment → Source** to **GitHub Actions** so the “Build & Deploy Jekyll (Hydejack)” workflow publishes the `_site` artifact instead of the legacy builder.
- To manually re-run the workflow, open the repository on GitHub, choose **Actions → Build & Deploy Jekyll (Hydejack)**, then use the `⋯` menu on the latest run and select **Re-run all jobs** (or click **Run workflow** for a clean trigger).
- Asset paths are case-sensitive on Linux runners; confirm the filenames in `_data` and template references match exactly (for example `GT.png` not `gt.png`) so logos like Georgia Tech render after deployment.
- If GitHub Actions fails during `bundle install` with a platform error (exit status 16), regenerate the lockfile on your Mac so it includes both the local and CI platforms:
  ```bash
  bundle lock --add-platform x86_64-linux
  bundle lock --add-platform arm64-darwin-23
  git add Gemfile.lock
  git commit -m "Add Linux and Apple Silicon platforms to Gemfile.lock"
  git push
  ```
  The Linux platform is required for GitHub-hosted runners; the Apple Silicon entry keeps the site working locally.

Feel free to extend this README with host-specific instructions, automation scripts, or content workflows as EmpiricalDefense grows.
