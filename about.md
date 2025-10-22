---
layout: about
image: /assets/img/headshot.png
description: >
  A boutique Jekyll theme for hackers, nerds, and academics,
  with a focus on personal sites that are meant to impress.
hide_description: true
redirect_from:
  - /download/
---

# About

<!--author-->


<!--more-->

1. this toc will be replaced by a hydejack-generated toc
{:toc}

## Education

<div class="education-grid">
  <article class="education-card">
    <img src="{{ '/assets/img/logos/gt.png' | relative_url }}" alt="Georgia Institute of Technology" loading="lazy" class="education-card__logo">
    <div class="education-card__body">
      <h3 class="education-card__degree">Ph.D., Electrical and Computer Engineering <span class="education-card__year">2022</span></h3>
      <p class="education-card__institution">Georgia Institute of Technology — Atlanta, GA</p>
      <p class="education-card__detail"><strong>Advisor:</strong> Dr. Brendan Saltaformaggio</p>
      <p class="education-card__detail"><strong>Dissertation:</strong> The Bot Reveals Its Master: Exposing and Infiltrating Botnet Command and Control Servers via Malware Logic Reuse</p>
    </div>
  </article>

  <article class="education-card">
    <img src="{{ '/assets/img/logos/afit.png' | relative_url }}" alt="Air Force Institute of Technology" loading="lazy" class="education-card__logo">
    <div class="education-card__body">
      <h3 class="education-card__degree">M.S., Computer Science <span class="education-card__year">2016</span></h3>
      <p class="education-card__institution">Air Force Institute of Technology — Dayton, OH</p>
      <p class="education-card__detail"><strong>Advisor:</strong> Dr. Benjamin Ramsey</p>
      <p class="education-card__detail"><strong>Thesis:</strong> A Misuse-Based Intrusion Detection System for ITU-T G.9959 Wireless Networks</p>
    </div>
  </article>

  <article class="education-card">
    <img src="{{ '/assets/img/logos/uab.png' | relative_url }}" alt="University of Alabama at Birmingham" loading="lazy" class="education-card__logo">
    <div class="education-card__body">
      <h3 class="education-card__degree">B.S., Computer Science <span class="education-card__year">2007</span></h3>
      <p class="education-card__institution">University of Alabama at Birmingham — Birmingham, AL</p>
    </div>
  </article>
</div>

## Professional Certifications <a class="about-cert-link" href="https://www.credly.com/users/jonathan-fuller.f869cdaf/badges#credly" target="_blank" rel="noopener">(Credly)</a>

<div class="certifications-grid">
  <article class="certification-card">
    <img src="{{ '/assets/img/certs/cissp.png' | relative_url }}" alt="(ISC)² CISSP" loading="lazy" class="certification-card__logo">
    <div class="certification-card__body">
      <h3 class="certification-card__title">Certified Information Systems Security Professional (CISSP)</h3>
      <p class="certification-card__issuer">(ISC)²</p>
    </div>
  </article>

  <article class="certification-card">
    <img src="{{ '/assets/img/certs/grem.png' | relative_url }}" alt="GIAC GREM" loading="lazy" class="certification-card__logo">
    <div class="certification-card__body">
      <h3 class="certification-card__title">GIAC Reverse Engineering Malware (GREM)</h3>
      <p class="certification-card__issuer">GIAC</p>
    </div>
  </article>

  <article class="certification-card">
    <img src="{{ '/assets/img/certs/gdsa.png' | relative_url }}" alt="GIAC GDSA" loading="lazy" class="certification-card__logo">
    <div class="certification-card__body">
      <h3 class="certification-card__title">GIAC Defensible Security Architecture (GDSA)</h3>
      <p class="certification-card__issuer">GIAC</p>
    </div>
  </article>

  <article class="certification-card">
    <img src="{{ '/assets/img/certs/gcfa.png' | relative_url }}" alt="GIAC GCFA" loading="lazy" class="certification-card__logo">
    <div class="certification-card__body">
      <h3 class="certification-card__title">GIAC Certified Forensic Analyst (GCFA)</h3>
      <p class="certification-card__issuer">GIAC</p>
    </div>
  </article>

  <article class="certification-card">
    <img src="{{ '/assets/img/certs/gsom.png' | relative_url }}" alt="GIAC GSOM" loading="lazy" class="certification-card__logo">
    <div class="certification-card__body">
      <h3 class="certification-card__title">GIAC Security Operations Manager (GSOM)</h3>
      <p class="certification-card__issuer">GIAC</p>
    </div>
  </article>
</div>

## Highlights

### Recent Insights

{% assign recent_posts = site.categories.posts | sort: 'date' | reverse %}
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

{% assign recent_publications = site.categories.publications | sort: 'date' | reverse %}
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
