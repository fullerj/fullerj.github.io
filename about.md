---
layout: about
image: /assets/img/headshot.png
description: >
  Chief Information Security Officer securing hybrid enterprise systems in adversarial environments.
  I build detection-driven security programs that improve visibility, accelerate response, and reduce systemic risk across enterprise-scale infrastructure, with a focus on emerging AI system security.
hide_description: true
redirect_from:
  - /download/
featured_writings:
  - /blog/ai-agent-control-validation-agent-bench/
  - /blog/spycraft-2-1-bitcoin-transaction-graph-analysis/
  - /blog/what-malware-persists/

selected_impact:
  - Lead cybersecurity strategy for a 10,000-user hybrid environment
  - Built detection engineering capability to expand threat coverage and reduce visibility gaps
  - Identified and disrupted 100+ adversary infrastructure abuse cases across global systems
  - Developed detection methods covering 100+ malware families and thousands of artifacts
  - Established early-stage AI security evaluation capability for agent-based systems


---

# About

{% unless page.hide_description %}
{{ page.description }}
{% endunless %}

<section class="home-section home-section--intro">


  <!--author-->

</section>

<section class="home-section home-section--impact" aria-label="Selected Impact">
  <div class="home-impact-box">
    <h3>Selected Impact</h3>
    <ul class="home-impact-list">
      {% for impact in page.selected_impact %}
      <li>{{ impact }}</li>
      {% endfor %}
    </ul>
  </div>
</section>

<!--more-->

{% assign featured_publications = '' | split: '' %}
{% if page.featured_publications and page.featured_publications.size > 0 %}
  {% for featured_url in page.featured_publications %}
    {% for p in site.posts %}
      {% if p.url == featured_url %}
        {% assign featured_publications = featured_publications | push: p %}
      {% endif %}
    {% endfor %}
  {% endfor %}
{% else %}
  {% assign featured_publications = site.posts | where_exp:'post','post.categories contains "publications"' | sort: 'date' | reverse %}
{% endif %}
{% assign media_mentions_urls = '' | split: '' %}

{% assign media_publications = site.posts | where_exp:'post','post.categories contains "publications"' %}
{% for pub in media_publications %}
  {% if pub.media_links %}
    {% for media in pub.media_links %}
      {% if media.url and media.url != '' %}
        {% unless media_mentions_urls contains media.url %}
          {% assign media_mentions_urls = media_mentions_urls | push: media.url %}
        {% endunless %}
      {% endif %}
    {% endfor %}
  {% endif %}
{% endfor %}

{% for talk in site.data.talks %}
  {% assign talk_media = talk.media_coverage %}
  {% if talk_media == nil or talk_media == empty %}
    {% assign talk_media = talk.media_links %}
  {% endif %}
  {% if talk_media == nil or talk_media == empty %}
    {% assign talk_media = talk.media %}
  {% endif %}

  {% if talk_media %}
    {% for media in talk_media %}
      {% if media.url and media.url != '' %}
        {% unless media_mentions_urls contains media.url %}
          {% assign media_mentions_urls = media_mentions_urls | push: media.url %}
        {% endunless %}
      {% endif %}
    {% endfor %}
  {% endif %}
{% endfor %}

## Recent Work

<section class="home-section home-section--highlights">
  <div class="home-highlights-grid">
    <article class="home-panel home-panel--feature" id="insights">
      <h3>Latest Strategy and Threats</h3>

{% assign recent_posts = site.posts | where_exp:'post','post.categories contains "posts"' %}
{% if recent_posts %}
  {% assign recent_posts = recent_posts | sort: 'date' | reverse %}
{% else %}
  {% assign recent_posts = '' | split: '' %}
{% endif %}

{% comment %}Select explicit featured writings (permalkinks) if provided in frontmatter{% endcomment %}
{% assign selected_writings = '' | split: '' %}
{% if page.featured_writings and page.featured_writings.size > 0 %}
  {% for featured_url in page.featured_writings %}
    {% for p in site.posts %}
      {% if p.url == featured_url %}
        {% assign selected_writings = selected_writings | push: p %}
      {% endif %}
    {% endfor %}
  {% endfor %}
{% endif %}

{% if selected_writings and selected_writings.size > 0 %}
  {% assign insights_posts = selected_writings %}
{% else %}
  {% assign insights_posts = recent_posts %}
{% endif %}
{% if insights_posts and insights_posts.size > 0 %}
<div class="home-posts-grid">
  {% for post in insights_posts limit:3 %}
  {% assign reading_wpm = site.blog.reading_wpm | default: 220 | plus: 0 %}
  {% if reading_wpm < 1 %}{% assign reading_wpm = 220 %}{% endif %}
  {% assign rounding_offset = reading_wpm | minus: 1 %}
  {% assign recent_word_count = post.content | strip_html | number_of_words %}
  {% assign recent_reading_minutes = recent_word_count | plus: rounding_offset | divided_by: reading_wpm %}
  {% if recent_reading_minutes < 1 %}{% assign recent_reading_minutes = 1 %}{% endif %}
  {% assign post_abstract = post.abstract | default: post.description | default: post.excerpt | strip_html | strip_newlines | truncate: 180 %}
  <article class="home-post-card" aria-expanded="false" data-flip-card>
    <div class="home-post-card__media">
      <div class="home-post-card__inner">
        <div class="home-post-card__face home-post-card__face--front">
          {% if post.blog_image %}
            <img src="{{ post.blog_image | relative_url }}" alt="{{ post.title }}" class="home-post-card__image" loading="lazy" decoding="async">
          {% elsif post.image and post.image.path %}
            <img src="{{ post.image.path | relative_url }}" alt="{{ post.image.alt }}" class="home-post-card__image" loading="lazy" decoding="async">
          {% endif %}
        </div>
        <div class="home-post-card__face home-post-card__face--back">
          <div class="home-post-card__back-content">
            {% if post_abstract and post_abstract != '' %}
            <p class="home-post-card__abstract">{{ post_abstract }}</p>
            {% endif %}
            <a class="home-post-card__read-more" href="{{ post.url | relative_url }}">Read more</a>
          </div>
        </div>
      </div>
    </div>
    <div class="home-post-card__content">
      <h4 class="home-post-card__title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h4>
      <small class="post-inline-meta">{{ recent_reading_minutes }} min read</small>
    </div>
  </article>
  {% endfor %}
</div>
<p><a href="{{ '/blog/' | relative_url }}">Read all analysis</a></p>
{% else %}
<p>Posts will appear here once they are published.</p>
{% endif %}

    </article>

    <article class="home-panel home-panel--feature" id="writing">
      <h3>Advisory and Leadership <small class="home-panel__source">(<a href="{{ '/service/' | relative_url }}">full roles & details</a>)</small></h3>

      <div class="service-cards">
        <details class="service-card">
          <summary>
            <span class="service-card__summary-main">
              <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/refractal.png' | relative_url }}');"></span>
              <span class="service-card__summary-copy">
                <span class="service-card__heading">Refractal</span>
                <span class="service-card__meta">Founding Advisor, Apr 2026-Present</span>
              </span>
            </span>
          </summary>
          <div class="service-card__content">
            <p>Advise <a href="https://refractal-ai.com">Refractal</a> on security strategy, adversarial risk, and control mechanisms for emerging systems, including agent-based architectures and runtime validation.</p>
          </div>
        </details>

        <details class="service-card">
          <summary>
            <span class="service-card__summary-main">
              <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/aiuc.jpeg' | relative_url }}');"></span>
              <span class="service-card__summary-copy">
                <span class="service-card__heading">AIUC-1</span>
                <span class="service-card__meta">Founding Consortium Member, Nov 2025-Present</span>
              </span>
            </span>
          </summary>
          <div class="service-card__content">
            <p>Contributing to the development of AIUC-1, the first end-to-end certification standard for enterprise AI agents, in collaboration with industry, government, academia, and nonprofits.</p>
          </div>
        </details>

        <details class="service-card">
          <summary>
            <span class="service-card__summary-main">
              <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/usenix-security-symposium.png' | relative_url }}');"></span>
              <span class="service-card__summary-copy">
                <span class="service-card__heading">USENIX Security Symposium</span>
                <span class="service-card__meta">Program Committee, 2024-Present</span>
              </span>
            </span>
          </summary>
          <div class="service-card__content">
            <p>Supporting peer review and technical program quality for the USENIX Security Symposium, a leading venue for systems and security research.</p>
          </div>
        </details>
      </div>

    </article>

    <article class="home-panel" id="talks">
      <h3>Speaking and Briefings</h3>

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
<p><a href="{{ '/talks/' | relative_url }}">View speaking and breifings</a></p>
{% else %}
<p>Talks will appear here once they are added to the site.</p>
{% endif %}

    </article>

    <article class="home-panel" id="media-coverage">
      <h3>Media and Mentions</h3>

{% assign recent_publications = site.posts | where_exp:'post','post.categories contains "publications"' | sort: 'date' | reverse %}
{% assign coverage_limit = 3 %}
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
<p><a href="{{ '/media/' | relative_url }}">View media mentions</a></p>
{% else %}
<p>Media highlights will appear here once coverage links are available.</p>
{% endif %}

    </article>

  </div>
</section>

<script>
(function () {
  var CARD_SELECTOR = '[data-flip-card]';

  function getCards(root) {
    return Array.prototype.slice.call(root.querySelectorAll(CARD_SELECTOR));
  }

  function isInteractiveTarget(target) {
    return !!target.closest('a, button, input, textarea, select, summary, label');
  }

  function setFlipped(card, flipped) {
    card.classList.toggle('is-flipped', flipped);
    card.setAttribute('aria-expanded', flipped ? 'true' : 'false');
  }

  function clearCards(root) {
    getCards(root).forEach(function (card) {
      setFlipped(card, false);
    });
  }

  function toggleCard(root, card) {
    var shouldFlip = !card.classList.contains('is-flipped');
    clearCards(root);
    setFlipped(card, shouldFlip);
  }

  function initFlipCards(root) {
    var cards = getCards(root);
    if (!cards.length) {
      return;
    }

    cards.forEach(function (card) {
      if (card.getAttribute('data-flip-initialized') === 'true') {
        return;
      }

      // Flip on hover (mouse) only. Clicking the card does nothing; only
      // the "Read more" link should navigate.
      card.addEventListener('mouseenter', function () {
        setFlipped(card, true);
      });

      card.addEventListener('mouseleave', function () {
        setFlipped(card, false);
      });

      // Allow Escape to close a flipped card when a keyboard user is within it.
      card.addEventListener('keydown', function (event) {
        if (event.key === 'Escape') {
          setFlipped(card, false);
        }
      });

      card.setAttribute('data-flip-initialized', 'true');
    });

    if (!root.hasAttribute('data-flip-global-initialized')) {
      root.addEventListener('click', function (event) {
        if (event.target.closest(CARD_SELECTOR)) {
          return;
        }

        clearCards(root);
      });

      root.addEventListener('keydown', function (event) {
        if (event.key === 'Escape') {
          clearCards(root);
        }
      });

      root.setAttribute('data-flip-global-initialized', 'true');
    }
  }

  initFlipCards(document);
})();
</script>

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
      <a class="about-cert-link" href="https://www.credly.com/users/jonathan-fuller.f869cdaf/badges#credly" target="_blank" rel="noopener noreferrer">Verify credentials on Credly</a>

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
