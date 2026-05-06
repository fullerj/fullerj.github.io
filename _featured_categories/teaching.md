---
layout: page
title: Practitioner Development
slug: teaching
description: >
  Development of cybersecurity practitioners through adversary-driven training, forensic investigation, and enterprise security scenarios.
hide_description: true

---

{% assign teaching_posts = site.posts | where_exp:'post','post.categories contains "teaching"' | sort: 'date' | reverse %}

<p><strong>Focused on adversary-driven training, detection engineering, and real-world incident investigation.</strong></p>

<p>Programs are designed to develop practitioners capable of operating in adversarial environments, translating technical findings into operational and executive decisions.</p>

<section class="teaching-group" aria-label="Practitioner Development Programs">
  <h2>Selected Courses</h2>

  {% if teaching_posts and teaching_posts.size > 0 %}
    <div class="teaching-expandables">
      {% for course in teaching_posts %}
        <details class="teaching-course">
          <summary>
            <span class="teaching-course__heading">
              {% if course.logo_url %}<span class="teaching-logo-mark" style="--teaching-logo: url('{{ course.logo_url | relative_url }}');"></span>{% endif %}
              {{ course.title }}{% if course.course_code %} ({{ course.course_code }}){% endif %}
            </span>
            {% if course.institution %}
            <span class="teaching-course__meta">{{ course.institution }}</span>
            {% endif %}
          </summary>

          <div class="teaching-course__content">
            {% if course.description %}
              <p class="teaching-course__description">{{ course.description }}</p>
            {% endif %}

            {% if course.location %}
              <p class="teaching-course__meta"><strong>Location:</strong> {{ course.location }}</p>
            {% endif %}
          </div>
        </details>
      {% endfor %}
    </div>
  {% else %}
    <p>No programs listed yet.</p>
  {% endif %}
</section>
