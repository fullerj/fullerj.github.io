---
layout: page
title: Teaching
slug: teaching
description: >
  University courses, guest lectures, and professional programs focused on cybersecurity and privacy.
---

{% assign teaching_posts = site.posts | where_exp:'post','post.categories contains "teaching"' | sort: 'date' | reverse %}
{% assign current_year = site.time | date: '%Y' | plus: 0 %}
{% assign recent_cutoff_year = current_year | minus: 2 %}

{% assign current_courses = '' | split: '' %}
{% assign recent_courses = '' | split: '' %}
{% assign past_courses = '' | split: '' %}

{% for course in teaching_posts %}
  {% assign course_year = course.date | date: '%Y' | plus: 0 %}
  {% if course_year >= current_year %}
    {% assign current_courses = current_courses | push: course %}
  {% elsif course_year >= recent_cutoff_year %}
    {% assign recent_courses = recent_courses | push: course %}
  {% else %}
    {% assign past_courses = past_courses | push: course %}
  {% endif %}
{% endfor %}

<section class="teaching-summary" aria-label="Teaching overview">
  <p class="teaching-summary__item"><strong>{{ teaching_posts | size }}</strong> total course entries</p>
  <p class="teaching-summary__item"><strong>{{ current_courses | size }}</strong> current</p>
  <p class="teaching-summary__item"><strong>{{ recent_courses | size }}</strong> recent</p>
  <p class="teaching-summary__item"><strong>{{ past_courses | size }}</strong> past</p>
</section>

<section class="teaching-logos">
  <h3 class="teaching-logos__heading">Institutions</h3>
  <ul class="teaching-logos__grid">
    <li class="teaching-logos__item">
      <img src="{{ '/assets/img/logos/usma.png' | relative_url }}" alt="United States Military Academy" loading="lazy" width="160" height="160">
      <span class="teaching-logos__label">United States Military Academy</span>
    </li>
    <li class="teaching-logos__item">
      <img src="{{ '/assets/img/logos/GT.png' | relative_url }}" alt="Georgia Institute of Technology" loading="lazy" width="160" height="160">
      <span class="teaching-logos__label">Georgia Institute of Technology</span>
    </li>
    <li class="teaching-logos__item">
      <img src="{{ '/assets/img/logos/umgc.png' | relative_url }}" alt="University of Maryland Global Campus" loading="lazy" width="160" height="160">
      <span class="teaching-logos__label">University of Maryland Global Campus</span>
    </li>
  </ul>
</section>

{% assign teaching_groups = "Current Teaching|current_courses,Recent Teaching|recent_courses,Past Teaching|past_courses" | split: ',' %}

{% for group in teaching_groups %}
  {% assign parts = group | split: '|' %}
  {% assign group_title = parts[0] %}
  {% assign group_key = parts[1] %}
  {% assign courses = nil %}
  {% if group_key == 'current_courses' %}
    {% assign courses = current_courses %}
  {% elsif group_key == 'recent_courses' %}
    {% assign courses = recent_courses %}
  {% else %}
    {% assign courses = past_courses %}
  {% endif %}

  <section class="teaching-group" aria-label="{{ group_title }}">
    <h2>{{ group_title }}</h2>
    {% if courses and courses.size > 0 %}
      <div class="teaching-expandables">
        {% for course in courses %}
          <details class="teaching-course">
            <summary>
              <span class="teaching-course__heading">{{ course.course_code | default: 'Course' }}: {{ course.title }}</span>
              <span class="teaching-course__meta">{{ course.term | default: course.date | date: '%Y' }}{% if course.institution %} - {{ course.institution }}{% endif %}</span>
            </summary>
            <div class="teaching-course__content">
              {% if course.description %}
                <p class="teaching-course__description">{{ course.description }}</p>
              {% endif %}
              <ul class="teaching-course__facts">
                {% if course.role %}<li><strong>Role:</strong> {{ course.role }}</li>{% endif %}
                {% if course.department %}<li><strong>Department:</strong> {{ course.department }}</li>{% endif %}
                {% if course.location %}<li><strong>Location:</strong> {{ course.location }}</li>{% endif %}
                {% if course.credits %}<li><strong>Credits:</strong> {{ course.credits }}</li>{% endif %}
              </ul>
              <p class="teaching-course__actions"><a href="{{ course.url | relative_url }}">View full course details</a></p>
            </div>
          </details>
        {% endfor %}
      </div>
    {% else %}
      <p>No courses listed in this section yet.</p>
    {% endif %}
  </section>
{% endfor %}
