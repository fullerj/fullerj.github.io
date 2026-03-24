---
layout: about
image: /assets/img/headshot.png
description: >
  Bridging research, education, and cyber defense.
hide_description: true
redirect_from:
  - /download/
---

# About

<section class="home-section home-section--intro">

<!--author-->

</section>

<!--more-->

{% assign insights_count = site.posts | where_exp:'post','post.categories contains "posts"' | size %}
{% assign research_count = site.posts | where_exp:'post','post.categories contains "publications"' | size %}
{% assign talks_count = site.data.talks | size %}
{% assign certifications_count = site.data.certifications | size %}

<section class="home-section home-section--snapshot">
  <div class="home-kpi-grid" role="list" aria-label="Executive snapshot">
    <article class="home-kpi" role="listitem">
      <p class="home-kpi__value">{{ insights_count }}</p>
      <p class="home-kpi__label">Insights</p>
    </article>
    <article class="home-kpi" role="listitem">
      <p class="home-kpi__value">{{ research_count }}</p>
      <p class="home-kpi__label">Research and Publications</p>
    </article>
    <article class="home-kpi" role="listitem">
      <p class="home-kpi__value">{{ talks_count }}</p>
      <p class="home-kpi__label">Talks and Events</p>
    </article>
    <article class="home-kpi" role="listitem">
      <p class="home-kpi__value">{{ certifications_count }}</p>
      <p class="home-kpi__label">Professional Certifications</p>
    </article>
  </div>

  <nav class="home-quick-links" aria-label="Quick access">
    <a class="home-quick-links__link" href="{{ '/blog/' | relative_url }}">Insights</a>
    <a class="home-quick-links__link" href="{{ '/publications/' | relative_url }}">Research</a>
    <a class="home-quick-links__link" href="{{ '/talks/' | relative_url }}">Speaking</a>
    <a class="home-quick-links__link" href="{{ '/service/' | relative_url }}">Service</a>
  </nav>
</section>

## Highlights

<section class="home-section home-section--highlights">
  <div class="home-highlights-grid">
    <article class="home-panel home-panel--feature">

      <h3>Recent Insights</h3>

{% assign recent_posts = site.posts | where_exp:'post','post.categories contains "posts"' %}
{% if recent_posts %}
  {% assign recent_posts = recent_posts | sort: 'date' | reverse %}
{% else %}
  {% assign recent_posts = '' | split: '' %}
{% endif %}
{% if recent_posts and recent_posts.size > 0 %}
<ul>
  {% for post in recent_posts limit:3 %}
  {% assign reading_wpm = site.blog.reading_wpm | default: 220 | plus: 0 %}
  {% if reading_wpm < 1 %}{% assign reading_wpm = 220 %}{% endif %}
  {% assign rounding_offset = reading_wpm | minus: 1 %}
  {% assign recent_word_count = post.content | strip_html | number_of_words %}
  {% assign recent_reading_minutes = recent_word_count | plus: rounding_offset | divided_by: reading_wpm %}
  {% if recent_reading_minutes < 1 %}{% assign recent_reading_minutes = 1 %}{% endif %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
    <br/>
    <small class="post-inline-meta">{{ recent_reading_minutes }} min read</small>
  </li>
  {% endfor %}
</ul>
<p><a href="{{ '/blog/' | relative_url }}">Browse all blog posts</a></p>
{% else %}
<p>Posts will appear here once they are published.</p>
{% endif %}

    </article>

    <article class="home-panel home-panel--feature">
      <h3>Research and Publications</h3>

{% assign recent_publications = site.posts | where_exp:'post','post.categories contains "publications"' %}
{% if recent_publications %}
  {% assign recent_publications = recent_publications | sort: 'date' | reverse %}
{% else %}
  {% assign recent_publications = '' | split: '' %}
{% endif %}
{% if recent_publications and recent_publications.size > 0 %}
<ul>
  {% for pub in recent_publications limit:3 %}
  <li>
    <strong><a href="{{ pub.url | relative_url }}">{{ pub.title }}</a></strong>
    {% if pub.conference %}
    <br/>
    <small>{{ pub.conference }}</small>
    {% endif %}
  </li>
  {% endfor %}
</ul>
<p><a href="{{ '/publications/' | relative_url }}">View all publications</a></p>
{% else %}
<p>Publications will appear here once they are added to the site.</p>
{% endif %}

    </article>

    <article class="home-panel">
      <h3>Recent Talks</h3>

{% assign recent_talks = site.data.talks | sort: 'date' | reverse %}
{% if recent_talks and recent_talks.size > 0 %}
<ul>
  {% for talk in recent_talks limit:3 %}
  <li>
    <strong>{{ talk.title }}</strong>
    {% capture talk_details %}
      {% if talk.event %}{{ talk.event }}{% endif %}
      {% if talk.event and talk.location %} - {% endif %}
      {% if talk.location %}{{ talk.location }}{% endif %}
    {% endcapture %}
    {% assign talk_details = talk_details | strip %}
    {% if talk_details != '' %}
    <br/>
    <small>{{ talk_details }}</small>
    {% endif %}
  </li>
  {% endfor %}
</ul>
<p><a href="{{ '/talks/' | relative_url }}">Explore talks and events</a></p>
{% else %}
<p>Talks will appear here once they are added to the site.</p>
{% endif %}

    </article>

    <article class="home-panel">
      <h3>Selected Media Coverage</h3>

{% assign coverage_limit = 6 %}
{% assign per_source_limit = 2 %}
{% assign publication_coverage = '' | split: '' %}
{% assign talk_coverage = '' | split: '' %}
{% assign coverage_seen_urls = '' | split: '' %}

{% if recent_publications %}
  {% for pub in recent_publications %}
    {% if pub.media_links %}
      {% assign sorted_pub_media = pub.media_links | sort: 'date' | reverse %}
      {% assign source_slot_count = 0 %}
      {% for media in sorted_pub_media %}
        {% if source_slot_count >= per_source_limit %}
          {% break %}
        {% endif %}
        {% if media.title and media.title != '' and media.url and media.url != '' %}
          {% assign media_url = media.url %}
          {% unless coverage_seen_urls contains media_url %}
            {% assign source_label = media.source %}
            {% if source_label == nil or source_label == '' %}
              {% assign normalized_url = media.url | replace: 'https://', '' | replace: 'http://', '' | replace: 'www.', '' %}
              {% assign source_label = normalized_url | split: '/' | first %}
            {% endif %}
            {% capture coverage_entry %}
<li>
  <strong><a href="{{ media.url }}">{{ media.title }}</a></strong>
  {% if source_label %}
  <br/>
  <small>{{ source_label }}</small>
  {% endif %}
</li>
            {% endcapture %}
            {% assign publication_coverage = publication_coverage | push: coverage_entry %}
            {% assign coverage_seen_urls = coverage_seen_urls | push: media_url %}
            {% assign source_slot_count = source_slot_count | plus: 1 %}
          {% endunless %}
        {% endif %}
      {% endfor %}
    {% endif %}
  {% endfor %}
{% endif %}

{% if recent_talks %}
  {% for talk in recent_talks %}
    {% assign talk_media = talk.media_coverage %}
    {% if talk_media == nil or talk_media == empty %}
      {% assign talk_media = talk.media_links %}
    {% endif %}
    {% if talk_media == nil or talk_media == empty %}
      {% assign talk_media = talk.media %}
    {% endif %}
    {% if talk_media %}
      {% assign sorted_talk_media = talk_media | sort: 'date' | reverse %}
      {% assign source_slot_count = 0 %}
      {% for media in sorted_talk_media %}
        {% if source_slot_count >= per_source_limit %}
          {% break %}
        {% endif %}
        {% if media.title and media.title != '' and media.url and media.url != '' %}
          {% assign media_url = media.url %}
          {% unless coverage_seen_urls contains media_url %}
            {% assign source_label = media.source %}
            {% if source_label == nil or source_label == '' %}
              {% assign normalized_url = media.url | replace: 'https://', '' | replace: 'http://', '' | replace: 'www.', '' %}
              {% assign source_label = normalized_url | split: '/' | first %}
            {% endif %}
            {% capture coverage_entry %}
<li>
  <strong><a href="{{ media.url }}">{{ media.title }}</a></strong>
  {% if source_label %}
  <br/>
  <small>{{ source_label }}</small>
  {% endif %}
</li>
            {% endcapture %}
            {% assign talk_coverage = talk_coverage | push: coverage_entry %}
            {% assign coverage_seen_urls = coverage_seen_urls | push: media_url %}
            {% assign source_slot_count = source_slot_count | plus: 1 %}
          {% endunless %}
        {% endif %}
      {% endfor %}
    {% endif %}
  {% endfor %}
{% endif %}

{% assign coverage_items = '' %}
{% assign coverage_count = 0 %}
{% assign pub_index = 0 %}
{% assign talk_index = 0 %}
{% assign pub_size = publication_coverage | size %}
{% assign talk_size = talk_coverage | size %}
{% assign max_iterations = coverage_limit | times: 4 %}
{% assign source_toggle = 'publication' %}

{% for i in (0..max_iterations) %}
  {% if coverage_count >= coverage_limit %}
    {% break %}
  {% endif %}
  {% assign entry = '' %}
  {% if source_toggle == 'publication' %}
    {% if pub_index < pub_size %}
      {% assign entry = publication_coverage[pub_index] %}
      {% assign pub_index = pub_index | plus: 1 %}
    {% elsif talk_index < talk_size %}
      {% assign entry = talk_coverage[talk_index] %}
      {% assign talk_index = talk_index | plus: 1 %}
    {% endif %}
    {% assign source_toggle = 'talk' %}
  {% else %}
    {% if talk_index < talk_size %}
      {% assign entry = talk_coverage[talk_index] %}
      {% assign talk_index = talk_index | plus: 1 %}
    {% elsif pub_index < pub_size %}
      {% assign entry = publication_coverage[pub_index] %}
      {% assign pub_index = pub_index | plus: 1 %}
    {% endif %}
    {% assign source_toggle = 'publication' %}
  {% endif %}

  {% if entry != '' %}
    {% assign coverage_items = coverage_items | append: entry %}
    {% assign coverage_count = coverage_count | plus: 1 %}
  {% endif %}

  {% if pub_index >= pub_size and talk_index >= talk_size %}
    {% break %}
  {% endif %}
{% endfor %}

{% if coverage_count > 0 %}
<ul>
{{ coverage_items }}
</ul>
{% else %}
<p>Media highlights will appear here once coverage links are available.</p>
{% endif %}

    </article>

    <article class="home-panel">
      <h3>Recent Service</h3>

{% assign service_posts = site.pages | where: 'url', '/service/' %}
{% if service_posts and service_posts.size > 0 %}
<ul>
  {% assign service_page = service_posts.first %}
  <li>
    <strong><a href="{{ service_page.url | relative_url }}">Community & Professional Service Highlights</a></strong><br/>
    <small>Volunteer outreach, mentoring, and reviewer engagements supporting the broader cybersecurity community.</small>
  </li>
</ul>
<p><a href="{{ '/service/' | relative_url }}">Explore service and volunteer efforts</a></p>
{% else %}
<p>Service highlights will appear here once they are published.</p>
{% endif %}

    </article>
  </div>
</section>

<section class="home-section home-section--credentials">
  <details class="home-expandable">
    <summary>Education</summary>

    <div class="home-expandable__content">
      <div class="education-grid">
  {% for edu in site.data.education %}
  <article class="education-card">
    {% if edu.logo %}
    <img src="{{ edu.logo | relative_url }}" alt="{{ edu.institution }}" loading="lazy" decoding="async" class="education-card__logo">
    {% endif %}
    <div class="education-card__body">
      <h3 class="education-card__degree">{{ edu.degree }}{% if edu.year %} <span class="education-card__year">{{ edu.year }}</span>{% endif %}</h3>
      <p class="education-card__institution">{{ edu.institution }}{% if edu.location %} - {{ edu.location }}{% endif %}</p>
      {% if edu.advisor %}
      <p class="education-card__detail"><strong>Advisor:</strong> {{ edu.advisor }}</p>
      {% endif %}
      {% if edu.dissertation %}
      <p class="education-card__detail"><strong>Dissertation:</strong> {{ edu.dissertation }}</p>
      {% endif %}
      {% if edu.thesis %}
      <p class="education-card__detail"><strong>Thesis:</strong> {{ edu.thesis }}</p>
      {% endif %}
    </div>
  </article>
  {% endfor %}
      </div>
    </div>
  </details>
</section>

<section class="home-section home-section--credentials">
  <details class="home-expandable">
    <summary>Professional Certifications</summary>

    <div class="home-expandable__content">
      <a class="about-cert-link" href="https://www.credly.com/users/jonathan-fuller.f869cdaf/badges#credly" target="_blank" rel="noopener">Verify credentials on Credly</a>

      <div class="certifications-grid">
  {% for cert in site.data.certifications %}
  <article class="certification-card">
    {% if cert.logo %}
    <img src="{{ cert.logo | relative_url }}" alt="{{ cert.alt | default: cert.title }}" loading="lazy" decoding="async" class="certification-card__logo">
    {% endif %}
    <div class="certification-card__body">
      <h3 class="certification-card__title">{{ cert.title }}</h3>
      {% if cert.issuer %}
      <p class="certification-card__issuer">{{ cert.issuer }}</p>
      {% endif %}
    </div>
  </article>
  {% endfor %}
      </div>
    </div>
  </details>
</section>
