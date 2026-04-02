---
layout: page
title: Media Coverage
permalink: /media/
description: >
  Interviews, features, and external coverage related to talks, technical writing, and research.
---

{% assign publications = site.posts | where_exp:'post','post.categories contains "publications"' | sort: 'date' | reverse %}
{% assign talks = site.data.talks | sort: 'date' | reverse %}

<section class="service-summary" aria-label="Media overview">
  <p class="service-summary__item"><strong>Publications</strong> media links collected from publication pages.</p>
  <p class="service-summary__item"><strong>Talks</strong> media links collected from talk entries.</p>
</section>

<nav class="service-quick-links" aria-label="Jump to media categories">
  <a href="#media-publications">Publication Coverage</a>
  <a href="#media-talks">Talk Coverage</a>
</nav>

<section class="service-group" id="media-publications" aria-label="Publication coverage">
  <h2>Publication Coverage</h2>
  {% assign publication_media_count = 0 %}
  <ul>
    {% for pub in publications %}
      {% if pub.media_links %}
        {% assign sorted_pub_media = pub.media_links | sort: 'date' | reverse %}
        {% for media in sorted_pub_media %}
          {% if media.title and media.title != '' and media.url and media.url != '' %}
            {% assign publication_media_count = publication_media_count | plus: 1 %}
            {% assign source_label = media.source %}
            {% if source_label == nil or source_label == '' %}
              {% assign normalized_url = media.url | replace: 'https://', '' | replace: 'http://', '' | replace: 'www.', '' %}
              {% assign source_label = normalized_url | split: '/' | first %}
            {% endif %}
            <li>
              <strong><a href="{{ media.url }}">{{ media.title }}</a></strong>
              <br/>
              <small>{{ source_label }}{% if media.date %} • {{ media.date | date: "%b %-d, %Y" }}{% endif %}</small>
              <br/>
              <small>Related publication: <a href="{{ pub.url | relative_url }}">{{ pub.title }}</a></small>
            </li>
          {% endif %}
        {% endfor %}
      {% endif %}
    {% endfor %}
  </ul>

  {% if publication_media_count == 0 %}
    <p>Publication coverage links will appear here as they are added.</p>
  {% endif %}
</section>

<section class="service-group" id="media-talks" aria-label="Talk coverage">
  <h2>Talk Coverage</h2>
  {% assign talk_media_count = 0 %}
  <ul>
    {% for talk in talks %}
      {% assign talk_media = talk.media_coverage %}
      {% if talk_media == nil or talk_media == empty %}
        {% assign talk_media = talk.media_links %}
      {% endif %}
      {% if talk_media == nil or talk_media == empty %}
        {% assign talk_media = talk.media %}
      {% endif %}

      {% if talk_media %}
        {% assign sorted_talk_media = talk_media | sort: 'date' | reverse %}
        {% for media in sorted_talk_media %}
          {% if media.title and media.title != '' and media.url and media.url != '' %}
            {% assign talk_media_count = talk_media_count | plus: 1 %}
            {% assign source_label = media.source %}
            {% if source_label == nil or source_label == '' %}
              {% assign normalized_url = media.url | replace: 'https://', '' | replace: 'http://', '' | replace: 'www.', '' %}
              {% assign source_label = normalized_url | split: '/' | first %}
            {% endif %}
            <li>
              <strong><a href="{{ media.url }}">{{ media.title }}</a></strong>
              <br/>
              <small>{{ source_label }}{% if media.date %} • {{ media.date | date: "%b %-d, %Y" }}{% endif %}</small>
              <br/>
              <small>Related talk: {{ talk.title }}{% if talk.event %} ({{ talk.event }}){% endif %}</small>
            </li>
          {% endif %}
        {% endfor %}
      {% endif %}
    {% endfor %}
  </ul>

  {% if talk_media_count == 0 %}
    <p>Talk coverage links will appear here as they are added.</p>
  {% endif %}
</section>
