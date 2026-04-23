---
layout: page
title: Leadership and Service
permalink: /service/
description: >
  Program committee work, peer review, consortium leadership, and outreach initiatives that advance the cybersecurity and trustworthy AI communities.
---

<section class="service-summary" aria-label="Service overview">
  <p class="service-summary__item"><strong>5</strong> active service roles</p>
  <p class="service-summary__item"><strong>1</strong> consortium leadership role</p>
  <p class="service-summary__item"><strong>4</strong> program committee appointments</p>
  <p class="service-summary__item"><strong>2</strong> journal and publication review engagements</p>
</section>

<nav class="service-quick-links" aria-label="Jump to service categories">
  <a href="#service-consortium">Security Standards Leadership</a>
  <a href="#service-committees">Program Committees</a>
  <a href="#service-review">Reviewer Engagements</a>
</nav>


<section class="service-group" id="service-consortium" aria-label="Security Standards Leadership">
  <h2>Security Standards Leadership</h2>
  <div class="service-cards">
    <details class="service-card">
      <summary>
        <span class="service-card__heading">AIUC-1</span>
        <span class="service-card__meta">Founding Consortium Member, Nov 2025-Present</span>
      </summary>
      <div class="service-card__content">
        <p>CContributing to the development of <a href="https://www.aiuc-1.com">AIUC-1</a>, the first end-to-end certification standard for enterprise AI agents, in collaboration with security, risk, and legal leaders across industry, government, academia, and nonprofits.</p>
        {% assign aiuc_artifacts = site.posts | where_exp: "item", "item.venue == 'AIUC-1' or item.authors contains 'AIUC-1' or item.tags contains 'AIUC' or item.tags contains 'AUIC'" | sort: "date" | reverse %}
        {% if aiuc_artifacts.size > 0 %}
          <p><strong>Related Artifacts</strong></p>
          <ul>
            {% for artifact in aiuc_artifacts %}
              <li>
                <a href="{{ artifact.url | relative_url }}">{{ artifact.title }}</a>
                {% if artifact.categories contains 'publications' %}(Publication){% else %}(Article){% endif %}
              </li>
            {% endfor %}
          </ul>
        {% endif %}
      </div>
    </details>
  </div>
</section>

<section class="service-group" id="service-committees" aria-label="Program Committee Membership">
  <h2>Program Committee Membership</h2>
  <div class="service-cards">
    <details class="service-card">
      <summary>
        <span class="service-card__heading">USENIX Security Symposium</span>
        <span class="service-card__meta">Program Committee, 2024-Present</span>
      </summary>
      <div class="service-card__content">
        <p>Supporting peer review and technical program quality for the <a href="https://www.usenix.org/conferences/byname/108">USENIX Security Symposium</a>, a leading venue for systems and security research.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__heading">Digital Forensics Research Workshop (DFRWS)</span>
        <span class="service-card__meta">Program Committee, 2024</span>
      </summary>
      <div class="service-card__content">
        <p>Contributed to review and selection of applied digital forensics research spanning academia, government, and industry practice.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__heading">International Conference on Cyber Warfare and Security (ICCWS)</span>
        <span class="service-card__meta">Program Committee, 2023-Present</span>
      </summary>
      <div class="service-card__content">
        <p>Evaluating research submissions and helping maintain program rigor for cyber defense and cyber conflict scholarship.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__heading">European Conference on Cyber Warfare and Security (ECCWS)</span>
        <span class="service-card__meta">Program Committee, 2023-2024</span>
      </summary>
      <div class="service-card__content">
        <p>Reviewed interdisciplinary work at the intersection of cyber operations, policy, and security strategy.</p>
      </div>
    </details>
  </div>
</section>

<section class="service-group" id="service-review" aria-label="Reviewer Engagements">
  <h2>Reviewer Engagements</h2>
  <div class="service-cards">
    <details class="service-card">
      <summary>
        <span class="service-card__heading">Computers &amp; Security (Elsevier Journal)</span>
        <span class="service-card__meta">Reviewer, 2022-Present</span>
      </summary>
      <div class="service-card__content">
        <p>Providing technical reviews for manuscripts covering applied cybersecurity research, threat analysis, and defensive engineering.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__heading">Cyber Defense Review</span>
        <span class="service-card__meta">Reviewer, 2024</span>
      </summary>
      <div class="service-card__content">
        <p>Reviewed submissions focused on operational cyber defense, strategy, and mission-relevant applications.</p>
      </div>
    </details>
  </div>
</section>
