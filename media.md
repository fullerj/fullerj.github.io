---
layout: page
title: In the Media
permalink: /media/
description: >
  Independent coverage of research on malware infrastructure, software supply-chain abuse,
  Zero Trust, and AI security, from The Hacker News, Communications of the ACM, USENIX ;login:, and more.

# Independent outlets shown in the "Featured in" strip, in display order.
featured_outlets:
  - The Hacker News
  - Communications of the ACM
  - "USENIX ;login:"
  - SiliconANGLE
  - Security Boulevard
  - ExecutiveGov
  - Tech Xplore
  - Tech Times

# Coverage lives on each publication's `media_links` (plus `media_name` / `media_hook`).
# A publication is placed in the theme whose `groups` contains its first link's `group`.
media_themes:
  - title: "Adversary Infrastructure & Malware"
    blurb: "How malware hides command-and-control inside legitimate web services, and how defenders and providers can find and take it down."
    groups: [adversary-ddr, adversary-web]
  - title: "Software Supply Chain"
    blurb: "Malicious and pirated plugins spreading through the WordPress ecosystem."
    groups: [software-ecosystem]
  - title: "AI Security & Governance"
    blurb: "What changes when AI moves from pilots into production."
    groups: [ai-security]
  - title: "Zero Trust & Enterprise Security"
    blurb: "Putting Zero Trust into practice inside Army systems and accreditation."
    groups: [ent-sec]
---

<!-- markdownlint-disable-file MD033 MD009 MD012 -->

{% assign publications = site.posts | where_exp: 'post', 'post.categories contains "publications"' | where_exp: 'post', 'post.media_links' | sort: 'date' | reverse %}

{% comment %} Totals across all coverage, de-duplicated by URL. {% endcomment %}
{% assign seen_urls = '' | split: '' %}
{% assign outlets = '' | split: '' %}
{% assign years = '' | split: '' %}
{% for pub in publications %}
  {% for m in pub.media_links %}
    {% if m.url and m.title %}
      {% unless seen_urls contains m.url %}
        {% assign seen_urls = seen_urls | push: m.url %}
        {% assign outlets = outlets | push: m.source %}
        {% assign y = m.date | default: pub.date | date: '%Y' %}
        {% assign years = years | push: y %}
      {% endunless %}
    {% endif %}
  {% endfor %}
{% endfor %}
{% assign outlets = outlets | compact | uniq %}
{% assign years = years | uniq | sort %}

<section class="media-hero" aria-label="Coverage overview">
  <p class="media-hero__lede">Research on how attackers abuse trusted platforms, and how to stop them, picked up by security press, academic outlets, and industry.</p>
  <dl class="media-stats">
    <div class="media-stat"><dt>Articles</dt><dd>{{ seen_urls.size }}</dd></div>
    <div class="media-stat"><dt>Outlets</dt><dd>{{ outlets.size }}</dd></div>
    <div class="media-stat"><dt>Research projects</dt><dd>{{ publications.size }}</dd></div>
    <div class="media-stat"><dt>Since</dt><dd>{{ years | first }}</dd></div>
  </dl>
  <p class="media-hero__outlets-label">Featured in</p>
  <ul class="media-hero__outlets">
    {% for o in page.featured_outlets %}<li>{{ o }}</li>{% endfor %}
  </ul>
</section>

{% for theme in page.media_themes %}
  {% assign theme_pubs = '' | split: '' %}
  {% for pub in publications %}
    {% assign first_group = pub.media_links[0].group %}
    {% if theme.groups contains first_group %}
      {% assign theme_pubs = theme_pubs | push: pub %}
    {% endif %}
  {% endfor %}
  {% if theme_pubs.size > 0 %}
<section class="media-theme" aria-labelledby="media-theme-{{ forloop.index }}">
  <h2 id="media-theme-{{ forloop.index }}">{{ theme.title }}</h2>
  <p class="media-theme__blurb">{{ theme.blurb }}</p>

  {% for pub in theme_pubs %}
    {% assign links = pub.media_links | sort: 'date', 'first' | reverse %}
    {% assign shown = '' | split: '' %}
  <article class="media-story">
    {% if pub.blog_image %}
    <a class="media-story__thumb" href="{{ pub.url | relative_url }}" tabindex="-1" aria-hidden="true">
      <img src="{{ pub.blog_image | relative_url }}" alt="" loading="lazy">
    </a>
    {% endif %}
    <div class="media-story__body">
      <p class="media-story__eyebrow">
        {% if pub.venue %}{{ pub.venue }} · {% endif %}{{ pub.date | date: '%Y' }}
      </p>
      <h3 class="media-story__title">
        <a href="{{ pub.url | relative_url }}">{% if pub.media_name %}<span class="media-story__name">{{ pub.media_name }}</span> {% endif %}<span class="media-story__full">{{ pub.title }}</span></a>
      </h3>
      {% if pub.media_hook %}<p class="media-story__hook">{{ pub.media_hook }}</p>{% endif %}

      <ul class="media-coverage">
        {% for m in links %}
          {% if m.url and m.title %}{% unless shown contains m.url %}
          {% assign shown = shown | push: m.url %}
          <li class="media-coverage__item">
            <a href="{{ m.url }}" target="_blank" rel="noopener noreferrer">
              <span class="media-coverage__outlet">{{ m.source | default: 'Coverage' }}</span>
              <span class="media-coverage__headline">{{ m.title }}</span>
            </a>
            {% if m.date %}<time class="media-coverage__date" datetime="{{ m.date | date: '%Y-%m-%d' }}">{{ m.date | date: '%b %Y' }}</time>{% endif %}
          </li>
          {% endunless %}{% endif %}
        {% endfor %}
      </ul>
    </div>
  </article>
  {% endfor %}
</section>
  {% endif %}
{% endfor %}

{% if publications.size == 0 %}
<p>Coverage links will appear here as they are added.</p>
{% endif %}

<section class="media-cta" aria-label="Media inquiries">
  <h2>Media &amp; speaking inquiries</h2>
  <p>Available for interviews, background briefings, and talks on malware infrastructure, AI security, and Zero Trust.</p>
  <p class="media-cta__links">
    <a href="https://www.linkedin.com/in/fullsec" target="_blank" rel="noopener noreferrer">Contact on LinkedIn</a>
    <a href="{{ '/talks/' | relative_url }}">See past talks</a>
  </p>
</section>
