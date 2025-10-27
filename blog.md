---
layout: page
title: Empirical Defense Blog
description: >
  Analyses, talk recaps, educational insights, and applied research highlights.
permalink: /blog/
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

<nav class="blog-archive-toc" aria-label="Blog archive">
  <h3 class="blog-archive-toc__heading">Browse by Year</h3>
  <ul>
    {% for year in posts_by_year %}
      <li><a href="#year-{{ year.name }}">{{ year.name }}</a></li>
    {% endfor %}
  </ul>
</nav>

{% if unique_tags.size > 0 %}
<section class="blog-tag-filter" aria-label="Filter posts by tag">
  <button type="button" class="blog-tag-filter__chip is-active" data-tag="__all">All posts</button>
  {% for tag in unique_tags %}
    <button type="button"
            class="blog-tag-filter__chip"
            data-tag="{{ tag | slugify }}">
      {{ tag }}
    </button>
  {% endfor %}
</section>
<p class="blog-selected-summary" aria-live="polite">Showing all posts</p>
<p class="blog-filter-hint">Tip: Click tags to toggle them; use “All posts” to clear.</p>
{% endif %}

{% for year in posts_by_year %}
### <span id="year-{{ year.name }}">{{ year.name }}</span>
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
  <article class="blog-card" data-tags="{{ post_tag_tokens | strip }}">
    <a class="blog-card__link" href="{{ post.url | relative_url }}">
      <div class="blog-card__image">
        {% if post.image %}
          {% include_cached components/hy-img.html img=post.image alt=post.title width=760 height=428 %}
        {% else %}
          <img src="{{ '/assets/img/logos/brand.png' | relative_url }}" alt="Empirical Defense icon" loading="lazy">
        {% endif %}
      </div>
      <div class="blog-card__body">
        <h2 class="blog-card__title">{{ post.title }}</h2>
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

<script>
(function () {
  var root = document;
  var galleries = Array.prototype.slice.call(root.querySelectorAll(".blog-gallery"));
  galleries.forEach(function (gallery) {
    var colsAttr = gallery.getAttribute("data-layout-columns");
    var columns = parseInt(colsAttr || "3", 10);
    if (isNaN(columns) || columns < 1) columns = 1;
    if (columns > 6) columns = 6;
    var className = "blog-gallery--cols-" + columns;
    [1,2,3,4,5,6].forEach(function (n) {
      gallery.classList.remove("blog-gallery--cols-" + n);
    });
    gallery.classList.add(className);
  });
})();
</script>
{% endfor %}

<script>
(function () {
  function initTagFilter(root) {
    var filter = root.querySelector('.blog-tag-filter');
    if (!filter || filter.hasAttribute('data-filter-initialized')) {
      return;
    }

    var buttons = Array.prototype.slice.call(filter.querySelectorAll('[data-tag]'));
    var articles = Array.prototype.slice.call(root.querySelectorAll('.blog-card'));
    if (!buttons.length || !articles.length) {
      return;
    }

    var tagLabels = {};
    buttons.forEach(function (btn) {
      var tagSlug = btn.getAttribute('data-tag');
      if (tagSlug && tagSlug !== '__all') {
        tagLabels[tagSlug] = btn.textContent.trim();
      }
    });

    var selected = new Set();

    function updateButtonStates() {
      buttons.forEach(function (btn) {
        var tag = btn.getAttribute('data-tag');
        var isAll = tag === '__all';
        var isActive = isAll ? selected.size === 0 : selected.has(tag);
        btn.classList.toggle('is-active', isActive);
        btn.setAttribute('aria-pressed', isActive ? 'true' : 'false');
      });
    }

    function applyFilter() {
      articles.forEach(function (article) {
        var tagsString = article.getAttribute('data-tags') || '';
        var tags = tagsString ? tagsString.split(/\s+/) : [];
        var matches = selected.size === 0 || tags.some(function (tag) {
          return selected.has(tag);
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
        if (heading && heading.tagName && heading.tagName.toLowerCase() === 'h3') {
          heading.hidden = isEmpty;
          heading.setAttribute('aria-hidden', isEmpty ? 'true' : 'false');
        }
      });

      updateSummary();
      highlightYears();
    }

    filter.addEventListener('click', function (event) {
      var button = event.target.closest('[data-tag]');
      if (!button) {
        return;
      }
      event.preventDefault();
      var tag = button.getAttribute('data-tag');
      var isAll = tag === '__all';
      var isActive = selected.has(tag);

      if (isAll) {
        selected.clear();
      } else {
        if (isActive) {
          selected.delete(tag);
        } else {
          selected.add(tag);
        }
      }

      updateButtonStates();
      applyFilter();
    });

    updateButtonStates();
    applyFilter();
    filter.setAttribute('data-filter-initialized', 'true');
  }


  function highlightYears() {
    galleries.forEach(function (gallery) {
      var heading = gallery.previousElementSibling;
      if (!heading) return;
      heading.classList.toggle('blog-archive-heading--active', !gallery.hidden);
    });
  }

  function updateSummary() {
    if (!summary) return;
    if (selected.size === 0) {
      summary.textContent = 'Showing all posts';
      return;
    }
    var friendly = Array.from(selected).map(function (tag) {
      return tagLabels[tag] || tag;
    }).sort();
    summary.textContent = 'Active tags: ' + friendly.join(', ');
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
      initTagFilter(document);
    });
  }
})();
</script>
