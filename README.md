# EmpiricalDefense Website

EmpiricalDefense is a Jekyll-powered site. The instructions below walk through the exact setup that works on macOS with `rbenv`, so you can build and preview the site locally before deploying it to your domain.

## macOS Prerequisites

- Install the Xcode Command Line Tools if you haven't already: `xcode-select --install`
- Install Homebrew from <https://brew.sh/> (optional but recommended)
- Install `rbenv` and the Ruby build tools:
  ```bash
  brew update
  brew install rbenv ruby-build
  ```
- Add `rbenv` to your shell once, then restart the terminal:
  ```bash
  echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
  exec zsh
  ```

## Local Setup

From the repository root (`/Users/jon/Repositories/empirical.github.io`):

1. Install and select the project Ruby (the version can be updated later if needed):
   ```bash
   rbenv install 3.2.3
   rbenv local 3.2.3
   rbenv rehash
   ruby -v
   ```
2. Install the Bundler version locked in `Gemfile.lock`:
   ```bash
   gem install bundler:2.2.3
   rbenv rehash
   ```
3. Keep gems inside the project and install dependencies:
   ```bash
   bundle config set --local path 'vendor/bundle'
   bundle _2.2.3_ install
   ```
4. Start the development server:
   ```bash
   bundle exec jekyll serve
   ```
5. Open <http://localhost:4000/> in your browser to preview EmpiricalDefense. The server rebuilds automatically when you edit project files.

## Content Structure

- Blog posts live in `blog/_posts/` and render via the main `Blog` navigation entry.
- Publications are grouped under the `Publications` sidebar link and sourced from `publications/_posts/`.
  * Each publication uses standard Jekyll front matter plus optional fields such as `conference`, `authors`, `acceptance_rate`, `pdf_url`, `slides_url`, `code_url`, `dataset_url`, `talk_url`, and `media_links`.
  * Copy `publications/_posts/2025-10-13-vader.md` as a template for new entries, replacing the placeholder resource links and BibTeX block.
  * Add a short `venue` string (e.g., `ACM CCS`) to prepend bracketed labels like `[ACM CCS]` on the publications index.
- Store self-hosted paper PDFs under `assets/papers/` and reference them with paths like `/assets/papers/my-paper.pdf` from publication front matter.
- Teaching continues to use the grouped-list layout via `_featured_categories/teaching.md` with entries in `teaching/_posts/` (`categories: [teaching]` for each file).
- Talks are curated on a single page (`talks.md`) which renders items from `_data/talks.yml`; add or edit YAML objects there to update the page.
- Featured category configuration lives in `_featured_categories/`. `Publications` is configured with the `list` layout so posts are grouped by year automatically.
- Global author metadata is stored in `_data/authors.yml`, while social links (including a Google Scholar stub) are managed in `_data/social.yml`.

## Next Steps

- Update site metadata in `_config.yml`, the navigation menu, and content pages (`index.html`, `about.md`, etc.) to match EmpiricalDefense branding.
- Replace placeholder publication resources (PDF, code, dataset, media coverage) with live links as they become available.
- When you are satisfied locally, deploy the generated `_site/` folder or connect the repository to your hosting platform (e.g., GitHub Pages) and point your domain to it.
- For DNS configuration with a custom domain, create the appropriate `CNAME` or `A` records once hosting is ready.

Feel free to expand this README with deployment-specific notes or additional tooling as EmpiricalDefense evolves.
