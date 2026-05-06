# Content Consistency Guide

This guide defines a consistent front matter schema and reusable includes for Publications and Teaching pages.

## Publications

Required front matter keys:
- `layout: publication`
- `title`
- `description` (1–2 sentences)
- `categories: [publications]`
- `date` (YYYY-MM-DD)

Recommended keys:
- `authors` (string or list)
- `venue` (conference/journal/institution)
- `doi` (just the DOI string)
- `pdf_url`, `code_url`, `video_url`
- `media_links` (list of `{ title, url, date }`)
- `image` (social preview / hero)
- `blog_image` (blog card image; falls back to a template image if omitted)
- `card_label` (visible badge on the blog card, e.g. `Article`, `Whitepaper`, `Publication`)
- `tags` (list)

Render consistent metadata by adding after your lead paragraph:

```
{% include components/publication-meta.html %}
```

## Teaching

Required front matter keys:
- `layout: post` (or `layout: teaching` if added)
- `title`
- `description` (1–2 sentences)
- `categories: [teaching]`

Recommended keys:
- `institution`, `department`
- `course_code` (e.g., `CS 483`)
- `term` (e.g., `Spring 2025`)
- `location`
- `role` (e.g., `Course Director`)
- `credits` (optional)
- `syllabus_url` (optional)
- `image` (social preview / hero)
- `tags` (list)

Render consistent metadata near the top of the page:

```
{% include components/teaching-meta.html %}
```

## Permalinks and slugs
- Use short permalinks for featured items (e.g., `/publications/operationalizing-zero-trust/`).
- Otherwise rely on the site default `/:categories/:year-:month-:day-:title/`.

## Authoring checklist
- Provide a clear `title`, `description`, and an `image` for better SEO/social cards.
- Move ad‑hoc metadata paragraphs (e.g., "Venue", "Authors") into front matter and the meta include to avoid duplication.
- Use `tags` and `categories` to enable search and filtering.

## Example front matter

Publication:
```yaml
---
layout: publication
title: "Operationalizing Zero Trust"
description: >
  Field collaboration validating Zero Trust mapping to DoD RMF.
categories: [publications]
date: 2025-09-11
authors: "F. Shah, Jonathan Fuller"
venue: "U.S. Army"
doi: 10.1000/example
pdf_url: https://example.com/paper.pdf
media_links:
  - title: "Coverage title"
    url: https://example.com/article
    date: "2025-09-15"
image: /assets/img/pubs/zero-trust.jpg
---
```

Teaching:
```yaml
---
layout: post
title: "CS 483: Digital Forensics"
description: >
  Undergraduate course examining evidence acquisition and analysis.
categories: [teaching]
institution: "United States Military Academy"
department: "EECS"
course_code: "CS 483"
term: "Spring 2025"
location: "West Point, NY"
role: "Course Director"
syllabus_url: https://example.com/syllabus.pdf
image: /assets/img/teaching/cs483-hero.jpg
---
```

## Migration tips
- Insert the appropriate include (`publication-meta.html` or `teaching-meta.html`) where you currently list metadata.
- Remove duplicated inline metadata paragraphs once the include renders correctly.
- Keep `authors` and `venue` in front matter for easier reuse across listings and cards.
