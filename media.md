---
layout: page
title: In the Media
permalink: /media/
description: >
  Interviews, features, and external coverage related to talks, technical writing, and research.
---

{% assign publications = site.posts | where_exp:'post','post.categories contains "publications"' | sort: 'date' | reverse %}
{% assign talks = site.data.talks | sort: 'date' | reverse %}
{% assign coverage_entries = '' | split: '' %}
{% assign coverage_outlets = '' | split: '' %}
{% assign coverage_years = '' | split: '' %}
{% assign coverage_seen_urls = '' | split: '' %}

{% for pub in publications %}
  {% if pub.media_links %}
    {% assign sorted_pub_media = pub.media_links | sort: 'date' | reverse %}
    {% for media in sorted_pub_media %}
      {% if media.title and media.title != '' and media.url and media.url != '' %}
        {% assign media_url = media.url %}
        {% unless coverage_seen_urls contains media_url %}
          {% assign source_label = media.source %}
          {% if source_label == nil or source_label == '' %}
            {% assign normalized_url = media.url | replace: 'https://', '' | replace: 'http://', '' | replace: 'www.', '' %}
            {% assign source_label = normalized_url | split: '/' | first %}
          {% endif %}
          {% assign coverage_outlets = coverage_outlets | push: source_label %}

          {% assign media_date = media.date | default: pub.date %}
          {% assign sort_key = media_date | date: "%Y-%m-%d" %}
          {% if sort_key and sort_key != '' and sort_key != '0000-00-00' %}
            {% assign coverage_years = coverage_years | push: sort_key | uniq %}
          {% endif %}

          {% capture coverage_entry %}
{{ sort_key }}|||<article class="media-card"> 
  <p class="media-card__eyebrow"><span class="media-badge">Publication</span> <span class="media-outlet">{{ source_label }}</span>{% if media_date %} <span class="media-date">{{ media_date | date: "%b %-d, %Y" }}</span>{% endif %}</p>
  <h3 class="media-card__title"><a href="{{ media.url }}" target="_blank" rel="noopener noreferrer" aria-label="Open media mention: {{ media.title }}">{{ media.title }}</a></h3>
  <p class="media-card__context">Related publication: <a href="{{ pub.url | relative_url }}">{{ pub.title }}</a></p>
  {% if media.related_talk and media.related_talk.title and media.related_talk.url %}
  <p class="media-card__context">Related talk: <a href="{{ media.related_talk.url | relative_url }}">{{ media.related_talk.title }}</a>{% if media.related_talk.event %} ({{ media.related_talk.event }}){% endif %}</p>
  {% endif %}
</article>
          {% endcapture %}
          {% assign coverage_entries = coverage_entries | push: coverage_entry %}
          {% assign coverage_seen_urls = coverage_seen_urls | push: media_url %}
        {% endunless %}
      {% endif %}
    {% endfor %}
  {% endif %}
{% endfor %}

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
        {% assign media_url = media.url %}
        {% unless coverage_seen_urls contains media_url %}
          {% assign source_label = media.source %}
          {% if source_label == nil or source_label == '' %}
            {% assign normalized_url = media.url | replace: 'https://', '' | replace: 'http://', '' | replace: 'www.', '' %}
            {% assign source_label = normalized_url | split: '/' | first %}
          {% endif %}
          {% assign coverage_outlets = coverage_outlets | push: source_label %}

          {% assign media_date = media.date | default: talk.date %}
          {% assign sort_key = media_date | date: "%Y-%m-%d" %}
          {% if sort_key and sort_key != '' and sort_key != '0000-00-00' %}
            {% assign coverage_years = coverage_years | push: sort_key | uniq %}
          {% endif %}

          {% capture coverage_entry %}
{{ sort_key }}|||<article class="media-card"> 
  <p class="media-card__eyebrow"><span class="media-badge">Talk</span> <span class="media-outlet">{{ source_label }}</span>{% if media_date %} <span class="media-date">{{ media_date | date: "%b %-d, %Y" }}</span>{% endif %}</p>
  <h3 class="media-card__title"><a href="{{ media.url }}" target="_blank" rel="noopener noreferrer" aria-label="Open media mention: {{ media.title }}">{{ media.title }}</a></h3>
  <p class="media-card__context">Related talk: {{ talk.title }}{% if talk.event %} ({{ talk.event }}){% endif %}</p>
  {% if media.related_publication and media.related_publication.title and media.related_publication.url %}
  <p class="media-card__context">Related publication: <a href="{{ media.related_publication.url | relative_url }}">{{ media.related_publication.title }}</a></p>
  {% endif %}
</article>
          {% endcapture %}
          {% assign coverage_entries = coverage_entries | push: coverage_entry %}
          {% assign coverage_seen_urls = coverage_seen_urls | push: media_url %}
        {% endunless %}
      {% endif %}
    {% endfor %}
  {% endif %}
{% endfor %}

{% assign coverage_entries = coverage_entries | sort | reverse %}
{% assign mentions_count = coverage_entries | size %}
{% assign outlets_count = coverage_outlets | uniq | size %}
{% assign sorted_years = coverage_years | sort %}
{% assign first_year = sorted_years | first | date: "%Y" %}
{% assign last_year = sorted_years | last | date: "%Y" %}

<section class="service-summary" aria-label="Media overview">
  <p class="service-summary__item"><strong>{{ mentions_count }}</strong> total mentions</p>
  <p class="service-summary__item"><strong>{{ outlets_count }}</strong> unique outlets</p>
  {% if first_year and first_year != '' and last_year and last_year != '' %}
    <p class="service-summary__item"><strong>{{ first_year }}-{{ last_year }}</strong> coverage span</p>
  {% endif %}
</section>

<section class="service-group" aria-label="All media coverage">
  <h2>Coverage</h2>
  {% if mentions_count > 0 %}
    <div class="media-timeline">
      {% for entry in coverage_entries %}
        {{ entry | split: '|||' | last }}
      {% endfor %}
    </div>
  {% else %}
    <p>Coverage links will appear here as they are added.</p>
  {% endif %}
</section>
