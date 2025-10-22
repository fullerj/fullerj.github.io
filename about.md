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

<!--author-->


<!--more-->



1. this toc will be replaced by a hydejack-generated toc
{:toc}

## Highlights

### Recent Insights

{% assign recent_posts = site.posts | where_exp:'post','post.categories contains "posts"' %}
{% if recent_posts %}
  {% assign recent_posts = recent_posts | sort: 'date' | reverse %}
{% else %}
  {% assign recent_posts = '' | split: '' %}
{% endif %}
{% if recent_posts and recent_posts.size > 0 %}
<ul>
  {% for post in recent_posts limit:3 %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong><br/>
    <small>{{ post.date | date: "%B %-d, %Y" }}</small>
  </li>
  {% endfor %}
</ul>
<p><a href="{{ '/blog/' | relative_url }}">Browse all blog posts</a></p>
{% else %}
<p>Posts will appear here once they are published.</p>
{% endif %}

### Recent Publications

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
    <strong><a href="{{ pub.url | relative_url }}">{{ pub.title }}</a></strong><br/>
    <small>{{ pub.date | date: "%B %-d, %Y" }}{% if pub.conference %} - {{ pub.conference }}{% endif %}</small>
  </li>
  {% endfor %}
</ul>
<p><a href="{{ '/publications/' | relative_url }}">View all publications</a></p>
{% else %}
<p>Publications will appear here once they are added to the site.</p>
{% endif %}

### Recent Talks

{% assign recent_talks = site.data.talks | sort: 'date' | reverse %}
{% if recent_talks and recent_talks.size > 0 %}
<ul>
  {% for talk in recent_talks limit:3 %}
  <li>
    <strong>{{ talk.title }}</strong><br/>
    <small>{{ talk.date | date: "%B %-d, %Y" }}{% if talk.event %} - {{ talk.event }}{% endif %}{% if talk.location %} - {{ talk.location }}{% endif %}</small>
  </li>
  {% endfor %}
</ul>
<p><a href="{{ '/talks/' | relative_url }}">Explore talks and events</a></p>
{% else %}
<p>Talks will appear here once they are added to the site.</p>
{% endif %}

### Recent Service

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

## Education

<div class="education-grid">
  {% for edu in site.data.education %}
  <article class="education-card">
    {% if edu.logo %}
    <img src="{{ edu.logo | relative_url }}" alt="{{ edu.institution }}" loading="lazy" class="education-card__logo">
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

## Professional Certifications 
<a class="about-cert-link" href="https://www.credly.com/users/jonathan-fuller.f869cdaf/badges#credly" target="_blank" rel="noopener">Verify credentials on Credly</a>

<div class="certifications-grid">
  {% for cert in site.data.certifications %}
  <article class="certification-card">
    {% if cert.logo %}
    <img src="{{ cert.logo | relative_url }}" alt="{{ cert.alt | default: cert.title }}" loading="lazy" class="certification-card__logo">
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
