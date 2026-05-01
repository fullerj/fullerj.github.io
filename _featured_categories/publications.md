---
layout: list
title: Technical Writing and Industry Papers
slug: publications
description: >
  A collection of technical writing and industry papers spanning multiple formats. This includes peer-reviewed research articles, online publications, and contributions across academic and professional venues.
---

{% assign pub_posts = site.categories.publications %}
{% if pub_posts %}
  {% assign pub_posts = pub_posts | sort: 'date' | reverse %}
{% else %}
  {% assign pub_posts = '' | split: '' %}
{% endif %}

{% assign pub_count = pub_posts | size %}
{% assign pub_years = '' | split: '' %}
{% assign pub_venues = '' | split: '' %}
{% assign all_pub_tags = '' | split: '' %}
{% for post in pub_posts %}
  {% assign year = post.date | date: "%Y" %}
  {% unless pub_years contains year %}
    {% assign pub_years = pub_years | push: year %}
  {% endunless %}
  {% if post.venue %}
    {% unless pub_venues contains post.venue %}
      {% assign pub_venues = pub_venues | push: post.venue %}
    {% endunless %}
  {% endif %}
  {% if post.tags %}
    {% assign all_pub_tags = all_pub_tags | concat: post.tags %}
  {% endif %}
{% endfor %}
{% assign unique_pub_tags = all_pub_tags | uniq | sort %}
{% assign pub_focus_groups = unique_pub_tags | group_by_exp: "tag", "tag | slice: 0, 1 | upcase" %}

<section class="pub-index-toolbar blog-index-toolbar" aria-label="Publications overview and filters">
  <p class="pub-index-toolbar__summary">
    <strong>{{ pub_count }}</strong> publications across <strong>{{ pub_years | size }}</strong> years{% if unique_pub_tags.size > 0 %} and <strong>{{ unique_pub_tags.size }}</strong> focus areas{% endif %}.
  </p>

  <div class="blog-index-toolbar__controls">
    {% if unique_pub_tags.size > 0 %}
      <div class="blog-focus-filter" aria-label="Filter publications by focus">
        <p class="blog-focus-filter__label">Filter by</p>
        <button type="button" class="blog-focus-filter__toggle" aria-expanded="false" aria-controls="pub-focus-filter-panel">
          <span class="blog-focus-filter__toggle-label">Focus Area</span>
          <span class="blog-focus-filter__toggle-caret" aria-hidden="true">▾</span>
        </button>
        <div id="pub-focus-filter-panel" class="blog-focus-filter__panel" hidden>
          <div class="blog-focus-filter__groups">
            {% for group in pub_focus_groups %}
              <div class="blog-focus-filter__group">
                <p class="blog-focus-filter__group-label">{{ group.name }}</p>
                <div class="blog-focus-filter__options">
                  {% for tag in group.items %}
                    <label class="blog-focus-filter__option">
                      <input type="checkbox" class="pub-focus-filter__checkbox" value="{{ tag | slugify }}">
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
      <label class="blog-sort-filter__label" for="pub-sort-select">Sort by</label>
      <select id="pub-sort-select" class="blog-sort-filter__select" aria-label="Sort publications">
        <option value="desc" selected>Newest first</option>
        <option value="asc">Oldest first</option>
      </select>
    </div>
  </div>

  {% if unique_pub_tags.size > 0 %}
    <p class="pub-selected-summary" aria-live="polite">Showing all publications</p>
    <p class="pub-filter-hint">Select a focus to narrow the archive.</p>
  {% endif %}
</section>

<script>
(function () {
  function initFocusDropdown(root) {
    var filter = root.querySelector('.blog-focus-filter');
    if (!filter) return;

    var toggle = filter.querySelector('.blog-focus-filter__toggle');
    var panel = filter.querySelector('.blog-focus-filter__panel');
    if (!toggle || !panel || toggle.hasAttribute('data-dropdown-initialized')) return;

    toggle.addEventListener('click', function () {
      var isExpanded = toggle.getAttribute('aria-expanded') === 'true';
      toggle.setAttribute('aria-expanded', isExpanded ? 'false' : 'true');
      panel.hidden = isExpanded;
    });

    toggle.setAttribute('data-dropdown-initialized', 'true');
  }

  initFocusDropdown(document);

  function initPublicationFilter(root) {
    var filter = root.querySelector('.blog-focus-filter');
    if (!filter || filter.hasAttribute('data-filter-initialized')) return;

    var checkboxes = Array.prototype.slice.call(filter.querySelectorAll('.pub-focus-filter__checkbox'));
    var items = Array.prototype.slice.call(root.querySelectorAll('.publication-list-item'));
    var groups = Array.prototype.slice.call(root.querySelectorAll('.publication-year-group'));
    var summary = root.querySelector('.pub-selected-summary');
    if (!checkboxes.length || !items.length) return;

    var tagLabels = {};
    checkboxes.forEach(function (checkbox) {
      var optionValue = checkbox.value;
      if (optionValue) {
        tagLabels[optionValue] = checkbox.nextElementSibling ? checkbox.nextElementSibling.textContent.trim() : optionValue;
      }
    });

    var selectedTags = new Set();

    function highlightYears() {
      groups.forEach(function (group) {
        var heading = group.querySelector('h2');
        if (!heading) return;
        var visibleItems = group.querySelector('.publication-list-item:not([hidden])');
        heading.classList.toggle('blog-archive-heading--active', !!visibleItems);
      });
    }

    function updateSummary() {
      if (!summary) return;
      if (selectedTags.size === 0) {
        summary.textContent = 'Showing all publications';
        return;
      }
      var focused = Array.from(selectedTags).map(function (tag) {
        return tagLabels[tag] || tag;
      }).sort();
      summary.textContent = 'Focused on: ' + focused.join(', ');
    }

    function applyFilter() {
      items.forEach(function (item) {
        var tagsString = item.getAttribute('data-tags') || '';
        var tags = tagsString ? tagsString.split(/\s+/) : [];
        var matches = selectedTags.size === 0 || tags.some(function (tag) {
          return selectedTags.has(tag);
        });
        item.hidden = !matches;
        item.setAttribute('aria-hidden', matches ? 'false' : 'true');
      });

      groups.forEach(function (group) {
        var visible = group.querySelector('.publication-list-item:not([hidden])');
        var heading = group.querySelector('h2');
        var isEmpty = !visible;
        group.hidden = isEmpty;
        group.setAttribute('aria-hidden', isEmpty ? 'true' : 'false');
        if (heading) {
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
      initPublicationFilter(document);
    }, { once: true });
  } else {
    initPublicationFilter(document);
  }

  var pushStateEl = document.getElementById('_pushState');
  if (pushStateEl) {
    pushStateEl.addEventListener('hy-push-state-after', function () {
      initPublicationFilter(document);
    });
  }
})();
</script>
