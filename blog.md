---
layout: page
title: Empirical Defense Blog
description: >
  Commentary and event recaps focused on evidence-driven cybersecurity leadership.
permalink: /blog/
---

Empirical Defense represents the philosophy that cybersecurity leadership should be grounded in evidence, education, and continuous learning. The platform explores how research, strategic governance, and education intersect to advance the art and science of cyber defense.

Through this lens, Empirical Defense promotes evidence-based approaches to leadership, resilience, and innovation in the evolving landscape of cyber threats. It serves as a space for advancing discussion, sharing insights, and connecting research with real-world security practice.

### Pillars

- **Research:** Ground decisions in data and analysis, not assumptions.
- **Education:** Share knowledge to strengthen teams and communities.
- **Leadership:** Convert insight into strategy, resilience, and measurable outcomes.

> **Mission:** EmpiricalDefense advances a future where cybersecurity decisions are informed by data, guided by research, and executed through principled leadership.

{% assign blog_posts = site.posts | where_exp:'post','post.categories contains "posts"' %}
{% if blog_posts %}
  {% assign blog_posts = blog_posts | sort: 'date' | reverse %}
{% else %}
  {% assign blog_posts = '' | split: '' %}
{% endif %}

{% assign posts_by_year = blog_posts | group_by_exp: "post", "post.date | date: '%Y'" %}

<nav class="blog-archive-toc" aria-label="Blog archive">
  <h3 class="blog-archive-toc__heading">Browse by Year</h3>
  <ul>
    {% for year in posts_by_year %}
      <li><a href="#year-{{ year.name }}">{{ year.name }}</a></li>
    {% endfor %}
  </ul>
</nav>

{% for year in posts_by_year %}
### <span id="year-{{ year.name }}">{{ year.name }}</span>
<section class="blog-gallery">
  {% for post in year.items %}
  <article class="blog-card">
    <a class="blog-card__link" href="{{ post.url | relative_url }}">
      <div class="blog-card__image">
        {% if post.image %}
          {% include_cached components/hy-img.html img=post.image alt=post.title width=760 height=428 %}
        {% else %}
          <img src="{{ '/assets/img/logos/brand.png' | relative_url }}" alt="EmpiricalDefense icon" loading="lazy">
        {% endif %}
      </div>
      <div class="blog-card__body">
        <h2 class="blog-card__title">{{ post.title }}</h2>
        {% if post.description %}
          <p class="blog-card__excerpt">{{ post.description }}</p>
        {% else %}
          <p class="blog-card__excerpt">{{ post.excerpt | strip_html | truncate: 140 }}</p>
        {% endif %}
      </div>
    </a>
  </article>
  {% endfor %}
</section>
{% endfor %}
