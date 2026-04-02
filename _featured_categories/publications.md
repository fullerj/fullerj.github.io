---
layout: list
title: Technical Writing and Industry Papers
slug: publications
description: >
  A collection of technical writing and industry papers spanning multiple formats. This includes peer-reviewed research articles, online publications, and contributions across academic and professional venues.
---

{% assign pub_posts = site.categories.publications %}
{% assign pub_count = pub_posts | size %}
{% assign pub_years = '' | split: '' %}
{% assign pub_venues = '' | split: '' %}
{% for post in pub_posts %}
  {% assign year = post.date | date: "%Y" %}
  {% unless pub_years contains year %}
    {% assign pub_years = pub_years | push: year %}
  {% endunless %}
  {% if post.venue %}
    {% unless pub_venues contains post.venue %}
      {% assign pub_venues = pub_venues | push: post.venue %}
    {% endunless %}
  {% endif %}
{% endfor %}

<section class="pub-index-toolbar" aria-label="Publications overview">
  <p class="pub-index-toolbar__summary">
    <strong>{{ pub_count }}</strong> publications across <strong>{{ pub_years | size }}</strong> years and <strong>{{ pub_venues | size }}</strong> venues.
  </p>
  <nav class="pub-index-toolbar__years" aria-label="Jump to publication year">
    {% for year in pub_years %}
      <a href="#y-{{ year }}">{{ year }}</a>
    {% endfor %}
  </nav>
</section>
