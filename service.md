---
layout: page
title: Leadership and Service
permalink: /service/
description: >
  Program committee work, peer review, consortium leadership, and outreach initiatives that advance the cybersecurity and trustworthy AI communities.
---

<!-- markdownlint-disable MD033 -->
<nav class="service-category-rail" aria-label="Jump to service categories">
  <a class="service-category-card service-category-card--advisory" href="#service-advisory">
    <span class="service-category-card__title">Advisory Roles</span>
    <span class="service-category-card__meta">1 role</span>
    <span class="service-category-card__logos" aria-hidden="true">
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/refractal.png' | relative_url }}');"></span>
    </span>
  </a>
  <a class="service-category-card service-category-card--consortium" href="#service-consortium">
    <span class="service-category-card__title">Security Standards</span>
    <span class="service-category-card__meta">1 consortium role</span>
    <span class="service-category-card__logos" aria-hidden="true">
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/aiuc.jpeg' | relative_url }}');"></span>
    </span>
  </a>
  <a class="service-category-card service-category-card--committees" href="#service-committees">
    <span class="service-category-card__title">Program Committees</span>
    <span class="service-category-card__meta">4 appointments</span>
    <span class="service-category-card__logos" aria-hidden="true">
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/usenix-security-symposium.png' | relative_url }}');"></span>
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/dfrws.png' | relative_url }}');"></span>
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/iccws.png' | relative_url }}');"></span>
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/eccws.png' | relative_url }}');"></span>
    </span>
  </a>
  <a class="service-category-card service-category-card--review" href="#service-review">
    <span class="service-category-card__title">Reviewer Engagements</span>
    <span class="service-category-card__meta">2 journal venues</span>
    <span class="service-category-card__logos" aria-hidden="true">
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/computers-and-security.png' | relative_url }}');"></span>
      <span class="service-logo-mark service-logo-mark--sm" style="--service-logo: url('{{ '/assets/img/service/cyber-defense-review.png' | relative_url }}');"></span>
    </span>
  </a>
</nav>

<section class="service-group service-group--advisory" id="service-advisory" aria-label="Advisory Roles">
  <h2>Advisory Roles</h2>
  <div class="service-cards">
    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/refractal.png' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">Refractal</span>
            <span class="service-card__meta">Founding Advisor, Apr 2026-Present</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Advise <a href="https://refractal-ai.com">Refractal</a> on security strategy, adversarial risk, and control mechanisms for emerging systems, including agent-based architectures and runtime validation. Contribute to early product direction and technical validation, including development of adversarial regulation mapping and runtime classification capabilities.</p>
      </div>
    </details>
  </div>
</section>

<section class="service-group service-group--consortium" id="service-consortium" aria-label="Security Standards Leadership">
  <h2>Security Standards Leadership</h2>
  <div class="service-cards">
    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/aiuc.jpeg' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">AIUC-1</span>
            <span class="service-card__meta">Founding Consortium Member, Nov 2025-Present</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Contributing to the development of <a href="https://www.aiuc-1.com">AIUC-1</a>, the first end-to-end certification standard for enterprise AI agents, in collaboration with security, risk, and legal leaders across industry, government, academia, and nonprofits.</p>
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

<section class="service-group service-group--committees" id="service-committees" aria-label="Program Committee Membership">
  <h2>Program Committee Membership</h2>
  <div class="service-cards">
    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/usenix-security-symposium.png' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">USENIX Security Symposium</span>
            <span class="service-card__meta">Program Committee, 2024-Present</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Supporting peer review and technical program quality for the <a href="https://www.usenix.org/conferences/byname/108">USENIX Security Symposium</a>, a leading venue for systems and security research.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/dfrws.png' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">Digital Forensics Research Workshop (DFRWS)</span>
            <span class="service-card__meta">Program Committee, 2024</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Contributed to review and selection of applied digital forensics research spanning academia, government, and industry practice.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/iccws.png' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">International Conference on Cyber Warfare and Security (ICCWS)</span>
            <span class="service-card__meta">Program Committee, 2023-Present</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Evaluating research submissions and helping maintain program rigor for cyber defense and cyber conflict scholarship.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/eccws.png' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">European Conference on Cyber Warfare and Security (ECCWS)</span>
            <span class="service-card__meta">Program Committee, 2023-2024</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Reviewed interdisciplinary work at the intersection of cyber operations, policy, and security strategy.</p>
      </div>
    </details>
  </div>
</section>

<section class="service-group service-group--review" id="service-review" aria-label="Reviewer Engagements">
  <h2>Reviewer Engagements</h2>
  <div class="service-cards">
    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/computers-and-security.png' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">Computers &amp; Security (Elsevier Journal)</span>
            <span class="service-card__meta">Reviewer, 2022-Present</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Providing technical reviews for manuscripts covering applied cybersecurity research, threat analysis, and defensive engineering.</p>
      </div>
    </details>

    <details class="service-card">
      <summary>
        <span class="service-card__summary-main">
          <span class="service-logo-mark" style="--service-logo: url('{{ '/assets/img/service/cyber-defense-review.png' | relative_url }}');"></span>
          <span class="service-card__summary-copy">
            <span class="service-card__heading">Cyber Defense Review</span>
            <span class="service-card__meta">Reviewer, 2024</span>
          </span>
        </span>
      </summary>
      <div class="service-card__content">
        <p>Reviewed submissions focused on operational cyber defense, strategy, and mission-relevant applications.</p>
      </div>
    </details>
  </div>
</section>
