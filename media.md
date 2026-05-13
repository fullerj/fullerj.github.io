---
layout: page
title: In the Media
permalink: /media/
description: >
  Selected external coverage and discussion of work on adversary behavior, detection engineering, and system security.
---

<!-- markdownlint-disable-file MD033 MD009 MD012 -->

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
          {% assign media_group = media.group | default: 'uncategorized' %}
          {% if sort_key and sort_key != '' and sort_key != '0000-00-00' %}
            {% assign coverage_years = coverage_years | push: sort_key | uniq %}
          {% endif %}

          {% capture coverage_entry %}
{{ sort_key }}|||{{ media_group }}|||{{ media.title }}|||{{ source_label }},{{ media_url }}
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
          {% assign media_group = media.group | default: 'uncategorized' %}
          {% if sort_key and sort_key != '' and sort_key != '0000-00-00' %}
            {% assign coverage_years = coverage_years | push: sort_key | uniq %}
          {% endif %}

          {% capture coverage_entry %}
{{ sort_key }}|||{{ media_group }}|||{{ media.title }}|||{{ source_label }},{{ media_url }}
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
{% if mentions_count > 0 %}

{% assign rendered_urls = '' | split: '' %}

<section class="service-group media-group media-group--adversary" aria-label="Adversary Infrastructure & Malware Research">
  <h2>Adversary Infrastructure & Malware Research</h2>
  <div class="media-main-box">
    <div class="media-sub-boxes">
      <article class="media-sub-box" aria-label="Dead-Drop Resolver Research and Detection Techniques">
        <h3>Dead-Drop Resolver Research and Detection Techniques</h3>
        <div class="media-timeline">
          {% assign rendered_titles_ddr = '' | split: '' %}
          {% for entry in coverage_entries %}
            {% assign parts = entry | split: '|||' %}
            {% assign entry_group = parts[1] %}
            {% assign entry_title = parts[2] %}
            {% assign source_pair = parts[3] %}
            {% if entry_group == 'adversary-ddr' %}
              {% unless rendered_titles_ddr contains entry_title %}
                <article class="media-card">
                  <h4 class="media-card__title">{{ entry_title }}</h4>
                  <div class="media-card__sources">
                    {% assign source_list = '' | split: '' %}
                    {% for check_entry in coverage_entries %}
                      {% assign check_parts = check_entry | split: '|||' %}
                      {% assign check_group = check_parts[1] %}
                      {% assign check_title = check_parts[2] %}
                      {% assign check_source = check_parts[3] %}
                      {% if check_group == 'adversary-ddr' and check_title == entry_title %}
                        {% assign source_list = source_list | push: check_source %}
                      {% endif %}
                    {% endfor %}
                    {% for source_pair in source_list %}
                      {% assign source_parts = source_pair | split: ',' %}
                      {% assign source_label = source_parts[0] %}
                      {% assign source_url = source_parts[1] %}
                      <a href="{{ source_url }}" target="_blank" rel="noopener noreferrer" class="media-outlet">{{ source_label }}</a>
                      {% unless forloop.last %}<span class="media-source-sep"> • </span>{% endunless %}
                    {% endfor %}
                  </div>
                </article>
                {% assign rendered_titles_ddr = rendered_titles_ddr | push: entry_title %}
              {% endunless %}
            {% endif %}
          {% endfor %}
        </div>
      </article>

      <article class="media-sub-box" aria-label="Malware Abuse of Cloud and Web Platforms">
        <h3>Malware Abuse of Cloud and Web Platforms</h3>
        <div class="media-timeline">
          {% assign rendered_titles_web = '' | split: '' %}
          {% for entry in coverage_entries %}
            {% assign parts = entry | split: '|||' %}
            {% assign entry_group = parts[1] %}
            {% assign entry_title = parts[2] %}
            {% assign source_pair = parts[3] %}
            {% if entry_group == 'adversary-web' %}
              {% unless rendered_titles_web contains entry_title %}
                <article class="media-card">
                  <h4 class="media-card__title">{{ entry_title }}</h4>
                  <div class="media-card__sources">
                    {% assign source_list = '' | split: '' %}
                    {% for check_entry in coverage_entries %}
                      {% assign check_parts = check_entry | split: '|||' %}
                      {% assign check_group = check_parts[1] %}
                      {% assign check_title = check_parts[2] %}
                      {% assign check_source = check_parts[3] %}
                      {% if check_group == 'adversary-web' and check_title == entry_title %}
                        {% assign source_list = source_list | push: check_source %}
                      {% endif %}
                    {% endfor %}
                    {% for source_pair in source_list %}
                      {% assign source_parts = source_pair | split: ',' %}
                      {% assign source_label = source_parts[0] %}
                      {% assign source_url = source_parts[1] %}
                      <a href="{{ source_url }}" target="_blank" rel="noopener noreferrer" class="media-outlet">{{ source_label }}</a>
                      {% unless forloop.last %}<span class="media-source-sep"> • </span>{% endunless %}
                    {% endfor %}
                  </div>
                </article>
                {% assign rendered_titles_web = rendered_titles_web | push: entry_title %}
              {% endunless %}
            {% endif %}
          {% endfor %}
        </div>
      </article>
    </div>
  </div>
</section>

<section class="service-group media-group" aria-label="AI Security & Emerging Systems">
  <h2>AI Security & Emerging Systems</h2>
  <div class="media-main-box">
    <div class="media-timeline">
      {% assign rendered_titles_ai = '' | split: '' %}
      {% for entry in coverage_entries %}
        {% assign parts = entry | split: '|||' %}
        {% assign entry_group = parts[1] %}
        {% assign entry_title = parts[2] %}
        {% assign source_pair = parts[3] %}
        {% if entry_group == 'ai-security' %}
          {% unless rendered_titles_ai contains entry_title %}
            <article class="media-card">
              <h4 class="media-card__title">{{ entry_title }}</h4>
              <div class="media-card__sources">
                {% assign source_list = '' | split: '' %}
                {% for check_entry in coverage_entries %}
                  {% assign check_parts = check_entry | split: '|||' %}
                  {% assign check_group = check_parts[1] %}
                  {% assign check_title = check_parts[2] %}
                  {% assign check_source = check_parts[3] %}
                  {% if check_group == 'ai-security' and check_title == entry_title %}
                    {% assign source_list = source_list | push: check_source %}
                  {% endif %}
                {% endfor %}
                {% for source_pair in source_list %}
                  {% assign source_parts = source_pair | split: ',' %}
                  {% assign source_label = source_parts[0] %}
                  {% assign source_url = source_parts[1] %}
                  <a href="{{ source_url }}" target="_blank" rel="noopener noreferrer" class="media-outlet">{{ source_label }}</a>
                  {% unless forloop.last %}<span class="media-source-sep"> • </span>{% endunless %}
                {% endfor %}
              </div>
            </article>
            {% assign rendered_titles_ai = rendered_titles_ai | push: entry_title %}
          {% endunless %}
        {% endif %}
      {% endfor %}
    </div>
  </div>
</section>

<section class="service-group media-group ent-sec" aria-label="Enterprise Security & Zero Trust">
  <h2>Enterprise Security & Zero Trust</h2>
  <div class="media-main-box">
    <div class="media-timeline">
      {% assign rendered_titles_ent = '' | split: '' %}
      {% for entry in coverage_entries %}
        {% assign parts = entry | split: '|||' %}
        {% assign entry_group = parts[1] %}
        {% assign entry_title = parts[2] %}
        {% assign source_pair = parts[3] %}
        {% if entry_group == 'ent-sec' %}
          {% unless rendered_titles_ent contains entry_title %}
            <article class="media-card">
              <h4 class="media-card__title">{{ entry_title }}</h4>
              <div class="media-card__sources">
                {% assign source_list = '' | split: '' %}
                {% for check_entry in coverage_entries %}
                  {% assign check_parts = check_entry | split: '|||' %}
                  {% assign check_group = check_parts[1] %}
                  {% assign check_title = check_parts[2] %}
                  {% assign check_source = check_parts[3] %}
                  {% if check_group == 'ent-sec' and check_title == entry_title %}
                    {% assign source_list = source_list | push: check_source %}
                  {% endif %}
                {% endfor %}
                {% for source_pair in source_list %}
                  {% assign source_parts = source_pair | split: ',' %}
                  {% assign source_label = source_parts[0] %}
                  {% assign source_url = source_parts[1] %}
                  <a href="{{ source_url }}" target="_blank" rel="noopener noreferrer" class="media-outlet">{{ source_label }}</a>
                  {% unless forloop.last %}<span class="media-source-sep"> • </span>{% endunless %}
                {% endfor %}
              </div>
            </article>
            {% assign rendered_titles_ent = rendered_titles_ent | push: entry_title %}
          {% endunless %}
        {% endif %}
      {% endfor %}
    </div>
  </div>
</section>

<section class="service-group media-group" aria-label="Software Ecosystem Security">
  <h2>Software Ecosystem Security</h2>
  <div class="media-main-box">
    <div class="media-timeline">
      {% assign rendered_titles_soft = '' | split: '' %}
      {% for entry in coverage_entries %}
        {% assign parts = entry | split: '|||' %}
        {% assign entry_group = parts[1] %}
        {% assign entry_title = parts[2] %}
        {% assign source_pair = parts[3] %}
        {% if entry_group == 'software-ecosystem' %}
          {% unless rendered_titles_soft contains entry_title %}
            <article class="media-card">
              <h4 class="media-card__title">{{ entry_title }}</h4>
              <div class="media-card__sources">
                {% assign source_list = '' | split: '' %}
                {% for check_entry in coverage_entries %}
                  {% assign check_parts = check_entry | split: '|||' %}
                  {% assign check_group = check_parts[1] %}
                  {% assign check_title = check_parts[2] %}
                  {% assign check_source = check_parts[3] %}
                  {% if check_group == 'software-ecosystem' and check_title == entry_title %}
                    {% assign source_list = source_list | push: check_source %}
                  {% endif %}
                {% endfor %}
                {% for source_pair in source_list %}
                  {% assign source_parts = source_pair | split: ',' %}
                  {% assign source_label = source_parts[0] %}
                  {% assign source_url = source_parts[1] %}
                  <a href="{{ source_url }}" target="_blank" rel="noopener noreferrer" class="media-outlet">{{ source_label }}</a>
                  {% unless forloop.last %}<span class="media-source-sep"> • </span>{% endunless %}
                {% endfor %}
              </div>
            </article>
            {% assign rendered_titles_soft = rendered_titles_soft | push: entry_title %}
          {% endunless %}
        {% endif %}
      {% endfor %}
    </div>
  </div>
</section>

<div class="media-timeline">
  {% assign rendered_titles_unc = '' | split: '' %}
  {% for entry in coverage_entries %}
    {% assign parts = entry | split: '|||' %}
    {% assign entry_group = parts[1] %}
    {% assign entry_title = parts[2] %}
    {% assign source_pair = parts[3] %}
    {% if entry_group == 'uncategorized' %}
      {% unless rendered_titles_unc contains entry_title %}
        <article class="media-card">
          <h4 class="media-card__title">{{ entry_title }}</h4>
          <div class="media-card__sources">
            {% assign source_list = '' | split: '' %}
            {% for check_entry in coverage_entries %}
              {% assign check_parts = check_entry | split: '|||' %}
              {% assign check_group = check_parts[1] %}
              {% assign check_title = check_parts[2] %}
              {% assign check_source = check_parts[3] %}
              {% if check_group == 'uncategorized' and check_title == entry_title %}
                {% assign source_list = source_list | push: check_source %}
              {% endif %}
            {% endfor %}
            {% for source_pair in source_list %}
              {% assign source_parts = source_pair | split: ',' %}
              {% assign source_label = source_parts[0] %}
              {% assign source_url = source_parts[1] %}
              <a href="{{ source_url }}" target="_blank" rel="noopener noreferrer" class="media-outlet">{{ source_label }}</a>
              {% unless forloop.last %}<span class="media-source-sep"> • </span>{% endunless %}
            {% endfor %}
          </div>
        </article>
        {% assign rendered_titles_unc = rendered_titles_unc | push: entry_title %}
      {% endunless %}
    {% endif %}
  {% endfor %}
</div>

{% else %}
  <section class="service-group" aria-label="All media coverage">
    <h2>Coverage</h2>
    <p>Coverage links will appear here as they are added.</p>
  </section>
{% endif %}
