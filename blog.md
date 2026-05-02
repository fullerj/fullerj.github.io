---
layout: page
title: Empirical Defense Blog
description: >
  Analysis, applied research, and field notes on cyber defense, adversary behavior, and AI security.
permalink: /blog/
hide_description: true
---

{% assign blog_posts = site.posts | where_exp:'post','post.categories contains "posts"' %}
{% if blog_posts %}
  {% assign blog_posts = blog_posts | sort: 'date' | reverse %}
{% else %}
  {% assign blog_posts = '' | split: '' %}
{% endif %}

{% assign posts_by_year = blog_posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% assign all_tags = '' | split: '' %}
{% for post in blog_posts %}
  {% if post.tags %}
    {% assign all_tags = all_tags | concat: post.tags %}
  {% endif %}
{% endfor %}
{% assign unique_tags = all_tags | uniq | sort %}
{% assign focus_groups = unique_tags | group_by_exp: "tag", "tag | slice: 0, 1 | upcase" %}
{% assign post_count = blog_posts | size %}

<section class="blog-hero" aria-label="Empirical Defense Blog">
  <div class="blog-hero__brand">
    <img class="blog-hero__logo" src="{{ '/assets/blogs/blog_logo.jpeg' | relative_url }}" alt="Empirical Defense blog logo" loading="eager" decoding="async">
    <div class="blog-hero__copy">
      <p class="blog-hero__lede">Empirical approaches to cyber defense, adversary behavior, and AI security, grounded in analysis, applied research, and real-world decision-making.</p>
    </div>
  </div>

  <div class="blog-hero__stats" aria-label="Blog summary">
    <p class="blog-hero__stat"><strong>{{ post_count }}</strong> posts</p>
    <p class="blog-hero__stat"><strong>{{ posts_by_year | size }}</strong> years</p>
    {% if unique_tags.size > 0 %}
      <p class="blog-hero__stat"><strong>{{ unique_tags.size }}</strong> focus areas</p>
    {% endif %}
  </div>

</section>

<section class="pub-index-toolbar blog-index-toolbar" aria-label="Blog overview and filters">
  <div class="blog-index-toolbar__controls">
    {% if unique_tags.size > 0 %}
      <div class="blog-focus-filter" aria-label="Filter posts by focus">
        <p class="blog-focus-filter__label">Filter by</p>
        <button type="button" class="blog-focus-filter__toggle" aria-expanded="false" aria-controls="blog-focus-filter-panel">
          <span class="blog-focus-filter__toggle-label">Focus Area</span>
          <span class="blog-focus-filter__toggle-caret" aria-hidden="true">▾</span>
        </button>
        <div id="blog-focus-filter-panel" class="blog-focus-filter__panel" hidden>
          <div class="blog-focus-filter__groups">
            {% for group in focus_groups %}
              <div class="blog-focus-filter__group">
                <p class="blog-focus-filter__group-label">{{ group.name }}</p>
                <div class="blog-focus-filter__options">
                  {% for tag in group.items %}
                    <label class="blog-focus-filter__option">
                      <input type="checkbox" class="blog-focus-filter__checkbox" value="{{ tag | slugify }}">
                      <span>{{ tag }}</span>
                    </label>
                  {% endfor %}
                </div>
              </div>
            {% endfor %}
          </div>
        </div>
      </div>
    {% endif %}

    <div class="blog-sort-filter">
      <label class="blog-sort-filter__label" for="blog-sort-select">Sort by</label>
      <select id="blog-sort-select" class="blog-sort-filter__select" aria-label="Sort blog posts">
        <option value="desc" selected>Newest first</option>
        <option value="asc">Oldest first</option>
      </select>
    </div>
  </div>

  {% if unique_tags.size > 0 %}
    <p class="blog-selected-summary" aria-live="polite">Showing all posts</p>
    <p class="blog-filter-hint">Select a focus to narrow the archive.</p>
  {% endif %}
</section>

<section class="blog-section blog-section--archive" aria-label="Blog archive by year">
  {% for year in posts_by_year %}
    <article class="blog-year-group" data-year="{{ year.name }}">
      <h2 class="blog-archive-heading" id="year-{{ year.name }}">{{ year.name }}</h2>
      <section class="blog-gallery" data-layout-columns="{{ site.blog.max_columns | default: 3 }}">
        {% for post in year.items %}
          {% assign post_tag_tokens = '' %}
          {% if post.tags %}
            {% for tag in post.tags %}
              {% assign tag_slug = tag | slugify %}
              {% if forloop.first %}
                {% assign post_tag_tokens = tag_slug %}
              {% else %}
                {% assign post_tag_tokens = post_tag_tokens | append: ' ' | append: tag_slug %}
              {% endif %}
            {% endfor %}
          {% endif %}
          <article class="blog-card" data-tags="{{ post_tag_tokens | strip }}" data-date="{{ post.date | date: '%Y-%m-%d' }}">
            <a class="blog-card__link" href="{{ post.url | relative_url }}">
              <div class="blog-card__image">
                {% if post.image %}
                  {% include_cached components/hy-img.html img=post.image alt=post.title width=760 height=428 %}
                {% else %}
                  <img src="{{ '/assets/img/logos/brand.png' | relative_url }}" alt="Empirical Defense icon" loading="lazy" decoding="async">
                {% endif %}
              </div>
              <div class="blog-card__body">
                <h3 class="blog-card__title">{{ post.title }}</h3>
                {% if post.description %}
                  <p class="blog-card__excerpt">{{ post.description }}</p>
                {% else %}
                  <p class="blog-card__excerpt">{{ post.excerpt | strip_html | truncate: 140 }}</p>
                {% endif %}
              </div>
            </a>
          </article>
        {% endfor %}
      </section>
    </article>
  {% endfor %}
</section>

<script>
(function () {
  var root = document;
  var galleries = Array.prototype.slice.call(root.querySelectorAll('.blog-gallery'));
  galleries.forEach(function (gallery) {
    var colsAttr = gallery.getAttribute('data-layout-columns');
    var columns = parseInt(colsAttr || '3', 10);
    if (isNaN(columns) || columns < 1) columns = 1;
    if (columns > 6) columns = 6;
    var className = 'blog-gallery--cols-' + columns;
    [1, 2, 3, 4, 5, 6].forEach(function (n) {
      gallery.classList.remove('blog-gallery--cols-' + n);
    });
    gallery.classList.add(className);
  });
})();
</script>

<script>
(function () {
  function sortArchive(root, sortOrder) {
    var archive = root.querySelector('.blog-section--archive');
    if (!archive) {
      return;
    }

    var yearGroups = Array.prototype.slice.call(archive.querySelectorAll('.blog-year-group'));
    if (!yearGroups.length) {
      return;
    }

    var ascending = sortOrder === 'asc';

    yearGroups.forEach(function (group) {
      var gallery = group.querySelector('.blog-gallery');
      if (!gallery) {
        return;
      }

      var cards = Array.prototype.slice.call(gallery.querySelectorAll('.blog-card'));
      cards.sort(function (left, right) {
        var leftDate = left.getAttribute('data-date') || '';
        var rightDate = right.getAttribute('data-date') || '';
        if (leftDate === rightDate) return 0;
        return ascending ? leftDate.localeCompare(rightDate) : rightDate.localeCompare(leftDate);
      });

      cards.forEach(function (card) {
        gallery.appendChild(card);
      });
    });

    yearGroups.sort(function (left, right) {
      var leftYear = parseInt(left.getAttribute('data-year') || '0', 10);
      var rightYear = parseInt(right.getAttribute('data-year') || '0', 10);
      return ascending ? leftYear - rightYear : rightYear - leftYear;
    });

    yearGroups.forEach(function (group) {
      archive.appendChild(group);
    });
  }

  function initSortControl(root) {
    var sortFilter = root.querySelector('.blog-sort-filter');
    if (!sortFilter) {
      return;
    }

    var select = sortFilter.querySelector('.blog-sort-filter__select');
    if (!select || select.hasAttribute('data-sort-initialized')) {
      return;
    }

    var applySort = function () {
      sortArchive(root, select.value || 'desc');
    };

    select.addEventListener('change', applySort);
    applySort();
    select.setAttribute('data-sort-initialized', 'true');
  }

  initSortControl(document);

  function initFocusDropdown(root) {
    var filter = root.querySelector('.blog-focus-filter');
    if (!filter) {
      return;
    }

    var toggle = filter.querySelector('.blog-focus-filter__toggle');
    var panel = filter.querySelector('.blog-focus-filter__panel');
    if (!toggle || !panel || toggle.hasAttribute('data-dropdown-initialized')) {
      return;
    }

    toggle.addEventListener('click', function () {
      var isExpanded = toggle.getAttribute('aria-expanded') === 'true';
      toggle.setAttribute('aria-expanded', isExpanded ? 'false' : 'true');
      panel.hidden = isExpanded;
    });

    toggle.setAttribute('data-dropdown-initialized', 'true');
  }

  initFocusDropdown(document);

  function initTagFilter(root) {
    var filter = root.querySelector('.blog-focus-filter');
    if (!filter || filter.hasAttribute('data-filter-initialized')) {
      return;
    }

    var checkboxes = Array.prototype.slice.call(filter.querySelectorAll('.blog-focus-filter__checkbox'));
    var articles = Array.prototype.slice.call(root.querySelectorAll('.blog-card'));
    var galleries = Array.prototype.slice.call(root.querySelectorAll('.blog-gallery'));
    var summary = root.querySelector('.blog-selected-summary');
    if (!checkboxes.length || !articles.length) {
      return;
    }

    var tagLabels = {};
    checkboxes.forEach(function (checkbox) {
      var optionValue = checkbox.value;
      if (optionValue) {
        tagLabels[optionValue] = checkbox.nextElementSibling ? checkbox.nextElementSibling.textContent.trim() : optionValue;
      }
    });

    var selectedTags = new Set();

    function highlightYears() {
      galleries.forEach(function (gallery) {
        var heading = gallery.previousElementSibling;
        if (!heading) return;
        heading.classList.toggle('blog-archive-heading--active', !gallery.hidden);
      });
    }

    function updateSummary() {
      if (!summary) return;
      if (selectedTags.size === 0) {
        summary.textContent = 'Showing all posts';
        return;
      }
      var focused = Array.from(selectedTags).map(function (tag) {
        return tagLabels[tag] || tag;
      }).sort();
      summary.textContent = 'Focused on: ' + focused.join(', ');
    }

    function applyFilter() {
      articles.forEach(function (article) {
        var tagsString = article.getAttribute('data-tags') || '';
        var tags = tagsString ? tagsString.split(/\s+/) : [];
        var matches = selectedTags.size === 0 || tags.some(function (tag) {
          return selectedTags.has(tag);
        });
        article.hidden = !matches;
        article.setAttribute('aria-hidden', matches ? 'false' : 'true');
      });

      galleries.forEach(function (gallery) {
        var visible = gallery.querySelector('.blog-card:not([hidden])');
        var heading = gallery.previousElementSibling;
        var isEmpty = !visible;
        gallery.hidden = isEmpty;
        gallery.setAttribute('aria-hidden', isEmpty ? 'true' : 'false');
        if (heading && heading.classList && heading.classList.contains('blog-archive-heading')) {
          heading.hidden = isEmpty;
          heading.setAttribute('aria-hidden', isEmpty ? 'true' : 'false');
        }
      });

      updateSummary();
      highlightYears();
    }

    checkboxes.forEach(function (checkbox) {
      checkbox.addEventListener('change', function () {
        if (checkbox.checked) {
          selectedTags.add(checkbox.value);
        } else {
          selectedTags.delete(checkbox.value);
        }
        applyFilter();
      });
    });

    applyFilter();
    filter.setAttribute('data-filter-initialized', 'true');
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', function () {
      initTagFilter(document);
    }, { once: true });
  } else {
    initTagFilter(document);
  }

  var pushStateEl = document.getElementById('_pushState');
  if (pushStateEl) {
    pushStateEl.addEventListener('hy-push-state-after', function () {
      initSortControl(document);
      initTagFilter(document);
    });
  }
})();
</script>

